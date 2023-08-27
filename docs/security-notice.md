# An important security notice

An important premise of LogSeq is that it is privacy-first and can be run entirely in offline mode, meaning you keep control of your data.

Sending data to your graph from mobile apps is a great way to quickly capture ideas and thoughts, but it also means that your data is being sent via multiple third-party services, including Telegram and WhatsApp, but also including LogLink itself.

For images and video, the file is also sent to imgbb which may publicly available.

While LogLink does not seek to collect any more data on you than is necessary (and seeks to be transparent about this by publishing its source code), using a third party services does come with inherent risks.

It is really important that if you use LogLink you understand the security implications of this, and that you are comfortable with the risks.

!!! danger "By using LogLink you are acknowledging that:"

    - This is a community project
    - It comes with absolutely no warranty as to performance or security
    - If your messages are not delivered or are compromised, LogLink is not responsible
    - By using LogLink you are releasing the authors and maintainers of LogLink from any liability for loss, damage, inconvenience or unhappiness if your data is lost or compromised


## How can I mitigate these risks?

You can't absolutely. Any time you send your data to a third party cloud service it is inherently open to compromise.

If you are concerned about the security implications of using LogLink, you can mitigate these risks by running your own version of the server, either under a virtual private server under your control or even locally. All of the code is available under open source and instructions for running it yourself are available [here](self-hosting.md).

## A *particularly* important note about images and video

Plain text and locations are processed locally on the server, but to deal with images and videos we use [imgBB](https://imgbb.com/), a third party service.

This means that images and videos you send to the server are uploaded to imgBB which may be publicly available.

## Data collected by LogLink

LogLink makes real efforts to collect as little data as possible about you, and store as little as possible. Below is the description of the data collected at each stage of the workflow.

!!! danger "The below is not a guarantee"

    Software can change, be compromised or not work as intended. The below is intended as a best efforts description of how the service is intended to function, but is not a guarantee that additional data may not be collected either through error or compromise.

### Overall workflow

To understand what data is potentially collected, it is useful to understand the overall workflow of LogLink and the various parts that comprise it:

- A **Messaging Service** such as Telegram or Whatsapp.

- **LogLink Server** is a simple web server running in the cloud, which receives data sent to it by Telegram and WhatsApp, stores it in a database under a unique token, and then delivers when that token is requested.

- **LogLink Plugin** is a simple plugin for LogLink which requests data from the server and populates it to your LogSeq graph.


### Step 1: Request a token

The user signs up to LogLink by sending a message to the LogLink bot on Telegram or WhatsApp.

This request is received by the **LogLink Server**. At this point a `user` record is created in the database on the server, containing the following records:

- `id`: this is a unique integer identifier for the record, which is used to identify the record in the database internally

- `token`: this is a long string of numbers and letters generated randomly at the time of the request

- `provider`: this identifies where the request came from (eg "whatsapp" or "telegram")

- `provider_id`: this is the unique identifier provided by the provider (whatsapp or telegram) to identify the user who made the request. This is:
    - For WhatsApp, the phone number of the user
    - For Telegram, the chat_id

- `approved`: this is a boolean value which is set to True by default. It is currently not in use, but might be used in the future to allow the service to blacklist users who are abusing the service.

- `api_call_count`: starts at 0, and is incremented each time a request is made to the API for this token. This is not used at present, but might be in future to identify users who are abusing the service.

The server then sends a message back to the user, containing the token.


### Step 2: Send data to the server

Subsequent messages sent to the LogLink bot are received by the **LogLink Server**. At this point the server checks which `provider` this was received from and the `provider_id` to identify which user the message came from.

Assuming the `provider_id` matches a user in the database, then it creates a new `message` record in the database, containing the following records:

- `id`: this is a unique integer identifier for the record, which is used to identify the record in the database internally. It does not relate to the user's `id`
- `provider`: this identifies where the request came from (eg "whatsapp" or "telegram")
- `provider_id`: this is the unique identifier provided by the provider (whatsapp or telegram) to identify the user who made the request - details as per the user record
- `provider_message_id`: this is the unique identifier provided by the provider (whatsapp or telegram) to identify the message. This is used for purposes such as marking the message as "read" when it is delivered to the user.
- `contents`: the contents of the message
- `timestamp`: the time the message was received by the server
- `delivered`: a boolean value which is set to False by default. It is set to True when the message is delivered to the user. In reality this field doesn't do much as, in its default configuration, the server will only deliver messages to the user once, and then delete them from the database. However it has been included in the database so that it can be used in future to allow the server to deliver messages multiple times, or to allow the user to request messages be delivered again.

Note that some Messaging Services (eg WhatsApp) provide additional information to the server (eg the name you have stored on the service, your avatar). LogLink doesn't need this data, so it doesn't seek to save it, but you should be aware that it is *available* to the server and in a situation where the server you are using is compromised, this data could be accessed.


### Step 3: Process the data

For some types of data received, there is some processing that takes place on the server side before the contents is saved to the database.

For plain text, no processing takes place.

For locations, the server parses the data received (eg latitude and longitude, address) to display this in a user-friendly way. All of this happens locally on the server.

For images and video, more processing happens which you should understand:

- The Server downloads the file from the Messaging Service, and saves it to a temporary location on the server
- The Server uploads the file to the cloud service
- The Server receives the URL of the file on the cloud service
- The Server stores the Imgur URL in its database
- The Server deletes the file from the temporary location on the server


### Step 4: Delivering the data to the client

The **LogLink Server** makes available an API which the **LogLink Plugin** can use to request data from the server.

When the plugin requests data from the server, it sends a request to the server, containing the token provided by the user.

On receiving the request, the server checks the token and if it is valid, it returns all of the messages stored in the database for the user associated with that token.

The server then seeks to delete the delivered messages from the database.


## Database storage

The messages, until they are delivered, are stored in a database on the server. This database is currently not encrypted, meaning that anyone who has access to the server (either a maintainer of the service, or someone who gains access in an unauthorised way) would be able to read the messages.

I do plan to encrypt the messages at rest in future, but this is not currently implemented.

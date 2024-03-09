# Self deployment

As explained in the [important security notice](/security-notice), sending messages through a service you do not control comes with inherent security risks. You can mitigate these risks by running your own server.

This guide explains how to deploy the code to your own server. Note that while this is doable, it does require knowledge of how to run your own server and some knowledge of Python. You will also have to learn how to set up Telegram/WhatsApp bot (which is not covered in this guide).

## What you will need

### Hardware

The server at [https://loglink.it](https://loglink.it/) runs on a Debian VPS, however you should be able to run the server from any VPS or even a machine you host locally. The instructions below are based on a Linux distribution, but there's no reason that this shouldn't also work on a Mac or Windows machine.

!!! note "Found the internet?"

    The server doesn't work well without constant internet access, so running it on a laptop or other machine which isn't constantly connected is a bad idea.

The code is not demanding to run so it should run happily on a Raspberry Pi or a low spec VPS. If you are looking for a low cost VPS then I use [hostworld.uk](https://hostworld.uk/) but have also had good experiences with [DigitalOcean](https://www.digitalocean.com/solutions/vps-hosting).

One important thing to note is that this will **not** deploy easily to service like Heroku. The database technology used is SQLite, which is robust and powerful but stores the data in a file. Services like Heroku use an ["emphemeral file system"](https://devcenter.heroku.com/articles/sqlite3) which means that the files are deleted and reloaded from their source on a periodic basis. This means that your database will be wiped at least 24 hours, which is not a good experience. If you do want to deploy to Heroku then you will need to modify the code to use an external MySQL or Postgres database server.

### A registered domain

If you plan to use Telegram then the Telegram bot will only function with a SSL certificate. The below guide talks you through how to get a free one, but this won't work if you're directing it to an IP address. You will need to purchase a web domain (any TLD will do) to direct this to.

## Setting up the server

This assumes that you have a functioning Linux server running with the following:

- an internet connection
- Python 3.7+
- Git

If you don't have that already then [this DigitalOcean guide](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04) is a great place to start.

### Clone the server repo

Clone the server repo to your local machine:

```bash
git clone https://github.com/hankhank10/loglink-server.git
```

### Install the required Python packages

Open the `loglink-server` directory and use pip to install the required packages:

```bash
pip3 install -r requirements.txt
```

### Set up your .env file

Your server will rely on various "secrets" in order to communicate securely with both itself and the various services that it will use. The secrets used for this server are not committed to the git repo (since then they wouldn't be secret!) so you will need to create your own.

The secrets should be stored in a file called `.env` in the root of the server directory. There is a file in the repo called `.envtemplate` which outlines the required variables. Rename this to `.env` and populate the fields with your own secrets.

#### Database (required)

The first thing we need to do is set up a secret key to secure our database. This can be anything you like but should be a long random string:

```py
APP_SECRET_KEY='alongr575243an45001domstring2that4nobodywouldguess3v3r'
```

#### Sentry credentials (optional)

Sentry is a handy tool for finding errors in production. It is free to sign up for and if you do want to use it them you should insert the "dsn" for your Sentry project in the below:

```py
SENTRY_DSN="https://4j8hfj009fe449977ee8j6e6@o.ingest.sentry.io/0395819423409380200"
```

Sentry  it's also optional for this repo. If you don't want to use sentry then you can leave the field here as `None` and Sentry will be disabled.

#### Admin credentials (required)

There is an admin dashboard made available at `/admin` which you can use to manage your server. You should set up a username and password for this dashboard by entering the following into your `.env` file:

```py
ADMIN_USERNAME='admin'
ADMIN_PASSWORD='alongpasswordn0bodyknows'
```

If you want to send automated onboarding emails to new users (optional) you will also need to set SMTP details:

```py
EMAIL_SETTING_HOST="smtp.yourdomain.com"
EMAIL_SETTING_PORT=587
EMAIL_SETTING_USERNAME="username"
EMAIL_SETTING_PASSWORD="password"
```

#### Telegram credentials (required)

You should create a new Telegram Bot [using the instructions here](https://core.telegram.org/bots/features#creating-a-new-bot).

You will be asked to create a name for the bot, which must end with "_bot".

!!! danger "A note on naming"

    Please do *not* include the name LogLink in your bot - your bot will be publicly available under this serarch term, and there is real potential for confusion if there are multiple bots with the same name.

The Botfather account will send you a token which you should enter here:

```py
TELEGRAM_BOT_NAME="the_name_you_chose_for_your_bot"
TELEGRAM_TOKEN="174329548:BBFjJaaBBG7EgCzABBgdfyGBYwX"
```

You should also choose a secret token of your own choice which Telegram will send to you when it sends you a message. This is to ensure that messages to your webhook are actually from Telegram:

```py
TELEGRAM_WEBHOOK_AUTH="asecretpasswordthatonlyyouknow"
```

You now need to tell the bot the location of the webhook that it should send messages to. You do this by sending a HTTP POST request (using Postman or [HTTPie](https://httpie.io/app)) with the following terms:

```
https://api.telegram.org/bot{your_bot_token}/setwebhook
    ?url={your_webhook_address}
    &secret_token={the_secret_token_of_your_choice}
```

In the above example, it would look like this:
```
https://api.telegram.org/bot174329548:BBFjJaaBBG7EgCzABBgdfyGBYwX/setwebhook?url=https://yourownapidomain.com/telegram/webhook/&secret_token=asecretpasswordthatonlyyouknow
```

You should get a 200 response like this:

```json
{
    "ok": true,
    "result": true,
    "description": "Webhook was set"
}
```

### Set up the database

There is a script provided that will set up the database for you. You can run it with:

```bash
bash db_reset
```

!!! danger "Careful!"

    Running `bash db_reset` will delete all the data in your database. If you have any data in there that you want to keep then you should back it up first.

### Run the server

Set debugging mode for the server to on by changing the bottom of `app.py` to:

```py
# Run the app
if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5011, debug=True)
```

Run the server in test mode with:

```bash
python3 app.py
```

### Deploy

Once you are happy your server is functioning as intended you can deploy it to a production server.

A really good guide to doing this is at [this tutorial in DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-gunicorn-and-nginx-on-ubuntu-22-04) although the same method will work regardless of where you are deploying it.

As mentioned above, Telegram won't work without a SSL certificate so make sure you also follow the certbot instructions in the tutorial.

Finally, before you deploy do remember to switch `debug=False` in your app.py file.

## Setting up the plugin

The plugin defaults to using the server at `https://api.loglink.it` but you can change this to your server in the plugin settings.

# Technologies used and how to contribute

LogLink consists of three separate code repos. Contributions are welcome on all of them.

All contributions are handled through GitHub. Please fork the repo, make your changes and then raise a pull request.

## Back end server

[:fontawesome-brands-github: View on Github](https://github.com/hankhank10/loglink-server){ .md-button-sm }

The server deals with all interactions with the Mobile Services, including onboarding new users and sending and receiving messages with the bots. The hosted version of this server runs at https://api.loglink.it/ but can also be deployed to your own server.

The server is written [Flask](https://flask.palletsprojects.com/en/2.2.x/), a Python web framework.

It uses the following packages and technologies:

- SQLite for database storage
- [Flask-SQLAlchemy](https://flask-sqlalchemy.palletsprojects.com/en/3.0.x/) for database ORM
- [Requests](https://requests.readthedocs.io/en/latest/) for making HTTP requests
- [Gunicorn](https://flask.palletsprojects.com/en/2.2.x/deploying/gunicorn/) for serving api responses
- [Sentry](https://sentry.io/) for error logging in production
- [Heyoo](https://github.com/Neurotech-HQ/heyoo) certain integrations with the WhatsApp API
- [imgur_python](https://pypi.org/project/imgur-python/) for dealing with the Imgur API

## LogSeq plugin

[:fontawesome-brands-github: View on Github](https://github.com/hankhank10/loglink-plugin){ .md-button-sm }

The plugin interacts with LogSeq to pull in data from the server for a given token.

The plugin is written in Javascript and uses the LogSeq plugin architecture.


## The documentation

[:fontawesome-brands-github: View on Github](https://github.com/hankhank10/loglink-docs){ .md-button-sm }

This documentation explains how LogLink works.

It is built in [MkDocs using the Material theme](https://squidfunk.github.io/mkdocs-material/).

Making changes to the documentation is easy - just edit the markdown files in the `docs` folder and then run `mkdocs serve` to see the changes locally.
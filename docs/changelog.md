# Changelogs

This lists the changes in the various releases to the server and the plugin. For the server, all changes are listed by date given it is a cloud service. For the plugin they are listed by the release versions posted to Github.

## Server 

[:material-github: Github link](https://github.com/hankhank10/logseq-server)

### 7 January 2023
- ✨Added functionality to check version number sent by plugin and display a warning if it is out of date - works with plugin v0.2.0
- ✨Automatically checks latest plugin version available based on Github release tag
- ✨Checks for latest plugin version every hour
- ✅Added basic tests with pytest

### 6 January 2023
- 📝Improve demo video
- 🐛Fixes issue around Telegram handler raising an error when it received an update that wasn't a message, fixing [this issue](https://github.com/hankhank10/loglink-server/issues/22)

### 5 January 2023
- 💄Better image for test uploads to imgbb, fixing [this issue](https://github.com/hankhank10/loglink-server/issues/17)
- 💄Fixes format of help message, fixing [this issue](https://github.com/hankhank10/loglink-server/issues/20)
- 💄Improved format of various user messages

### 4 January 2023

- 🚀Initial release for private alpha


## Plugin

[:material-github: Github link](https://github.com/hankhank10/logseq-plugin)

### Unreleased but committed to repo [:material-github:](https://github.com/hankhank10/loglink-plugin/)
- None as yet

### v0.2.0 (7 January 2023) [:material-github:](https://github.com/hankhank10/loglink-plugin/releases/tag/v0.2.0)
- ✨Now sends version number to server, so that the server can send a message to the user if it is out of date
- ✨Adds option for user to opt out of sending version number to server

### v0.1.7 (6 January 2023) [:material-github:](https://github.com/hankhank10/loglink-plugin/releases/tag/v0.1.7)
- 🚀Add to Logseq plugin marketplace
- 📝Update plugin description
- 📦️Change release to no longer reflect pre-release
- 📝Improved readme

### v0.1.5 (5 January 2023) [:material-github:](https://github.com/hankhank10/loglink-plugin/releases/tag/v0.1.5)

- 🔖Correct error in version number
- 📝Update readme and updated demo
- 👷Update workflows to produce build in preparation for submitting to marketplace

### v0.1.0 (4 January 2023) [:material-github:](https://github.com/hankhank10/loglink-plugin/releases/tag/v0.1.0)

- 🚀Initial release, available only via Github download and requires manual unpack
- 
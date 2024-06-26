# Changelogs

This lists the changes in the various releases to the server and the plugin. For the server, all changes are listed by date given it is a cloud service. For the plugin they are listed by the release versions posted to Github.

## Server 

[:material-github: Github link](https://github.com/hankhank10/logseq-server)

### 25 April 2024
- 🧪 Left beta

### 26 December 2023
- ✨ Released beta of Obsidian plugin

### 24 December 2023
- ✨ Added ability to produce dummy data for easier testing
- ✨ Built functionality to support Obsidian plugin

### 22 December 2023
- ✅ Built admin console
- ✅ Built onboarding process to remove manual steps
- ✅ Launched beta to users
- 😢 Sadly had to delete and reset database to allow new features

### 4 September 2023
- ✅ Checks that LogLink works with [Beeper](https://www.beeper.com/) via Telegram - it does 🎉

#### 27 August 2023
- 🗃️ Various backend changes ahead of beta launch
- 🥅 More robust error handling ahead of beta launch

#### 22 January 2023
- 🐛 Stops message being reported as an error in bugs
- 🐛 Fixes [this issue](https://github.com/hankhank10/loglink-server/issues/28) which meant when a user sent a document they were repeatedly told the message could not be delivered - thanks to [dwrhodes](https://github.com/dwrhodes) for reporting the bug
- 📝 Prettified this changelog

#### 15 January 2023
- ✅ Added full workflow testing with pytest before commits

#### 14 January 2023
- 🗃️ Added function to assist with database testing

#### 8 January 2023
- 🐛 Fixes an issue which caused the server to crash after refactoring the codebase
- 🙈 Remove the `media_to_use` folder which, ironically, was not being used

#### 7 January 2023
- ✨ Added functionality to check version number sent by plugin and display a warning if it is out of date - works with plugin v0.2.0
- ✨ Automatically checks latest plugin version available based on Github release tag
- ✨ Checks for latest plugin version every hour
- ♻️ Refactored code structure to make it testable with pytest
- ✅ Added basic tests with pytest

#### 6 January 2023
- 📝 Improve demo video
- 🐛 Fixes issue around Telegram handler raising an error when it received an update that wasn't a message, fixing [this issue](https://github.com/hankhank10/loglink-server/issues/22)

#### 5 January 2023
- 💄 Better image for test uploads to imgbb, fixing [this issue](https://github.com/hankhank10/loglink-server/issues/17)
- 💄 Fixes format of help message, fixing [this issue](https://github.com/hankhank10/loglink-server/issues/20)
- 💄 Improved format of various user messages

#### 4 January 2023
- 🚀 Initial release for private alpha


## Plugin

[:material-github: Github link](https://github.com/hankhank10/logseq-plugin)

### Unreleased but committed to repo [:material-github:](https://github.com/hankhank10/loglink-plugin/)
- None as yet

### v0.2.0 (7 January 2023) [:material-github:](https://github.com/hankhank10/loglink-plugin/releases/tag/v0.2.0)
- ✨ Now sends version number to server, so that the server can send a message to the user if it is out of date
- ✨ Adds option for user to opt out of sending version number to server

### v0.1.7 (6 January 2023) [:material-github:](https://github.com/hankhank10/loglink-plugin/releases/tag/v0.1.7)
- 🚀 Add to Logseq plugin marketplace
- 📝 Update plugin description
- 📦️ Change release to no longer reflect pre-release
- 📝 Improved readme

### v0.1.5 (5 January 2023) [:material-github:](https://github.com/hankhank10/loglink-plugin/releases/tag/v0.1.5)
- 🔖 Correct error in version number
- 📝 Update readme and updated demo
- 👷 Update workflows to produce build in preparation for submitting to marketplace

### v0.1.0 (4 January 2023) [:material-github:](https://github.com/hankhank10/loglink-plugin/releases/tag/v0.1.0)
- 🚀 Initial release, available only via Github download and requires manual unpack


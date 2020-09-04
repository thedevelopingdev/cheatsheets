# Bugs and gotchas cheatsheet

## Linux

### Spotify
* Indications: Spotify track changes are laggy and the UI becomes unresponsive to user input.
* Diagnosis: This is actually because Spotify is not able to send notifications to the desktop, and the timeout is locking the UI.
* Remedy: On Ubuntu 18.04, install the `notify-osd` package to enable desktop notifications.
# macOS configuration

```bash
# prevent DHCP from changing macOS hostname
# source: https://apple.stackexchange.com/questions/272036/how-to-refuse-dhcp-server-to-change-my-hostname
sudo scutil --set HostName yourcomputername
sudo scutil --set LocalHostName yourcomputername
sudo scutil --set ComputerName "Your Computer name"
```

```bash
# disable accent popup when long-pressing a key
# source: https://apple.stackexchange.com/questions/332769/macos-disable-popup-showing-accented-characters-when-holding-down-a-key
defaults write -g ApplePressAndHoldEnabled -bool false
```

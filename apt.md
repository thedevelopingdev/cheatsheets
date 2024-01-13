# apt

```bash
# sources:
# - https://askubuntu.com/questions/1286545/what-commands-exactly-should-replace-the-deprecated-apt-key/1307181#1307181

# Adding a repository
vim /etc/apt/sources.list.d/debian.list

# add the following in the file:
# deb http://deb.debian.org/debian buster main

# sudo apt update to find required public keys
sudo apt-get update

# add keys
gpg --keyserver KEYSERVER --no-default-keyring --keyring ./KEYRING.gpg --recv-keys LIST_OF_KEYS
sudo mv ./KEYRING.gpg /etc/apt/keyrings/KEYRING.gpg

# modify the above debian.list file with the keyring
# deb [signed-by=/etc/apt/keyrings/KEYRING.gpg] http://deb.debian.org/debian buster main
```

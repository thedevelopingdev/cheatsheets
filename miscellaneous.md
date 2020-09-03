find /var/www/html -type d -print0 | xargs -0 chmod 755 
find /var/www/html -type f -print0 | xargs -0 chmod 644

Pip install from Github repository: https://adamj.eu/tech/2019/03/11/pip-install-from-a-git-repository/

python -m pip install git+https://github.com/user/repo.git@commit

Set default application:
```bash
$ xdg-mime default <application>.desktop <mimetype>
```

Find current default application for mimetype
```bash
$ xdg-mime query default <mimetype>
```

Find mimetype of file
```bash
$ xdg-mime query filetype <filename>
```

Change volume
```bash
$ pactl set-sink-volume 1 <delta>%
```

## Important files
* `/etc/debian_version`. Find the Debian version that Ubuntu is based on.

## C Header files
* `unistd.h`. Header file that provides access to the POSIX operating system API.

## Shell redirection
`2>&1`; redirect file descriptor 2 to reference to file descriptor 1.

`/lib/systemd/system/`: location of `systemctl` service files.
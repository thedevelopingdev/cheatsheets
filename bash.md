# Bash programming cheatsheet

- Give variables default values

```
VARIABLE=${1:-<default val>}
```

- View which processes are bound to which ports

```sh
netstat -tulpn | grep :<port>
ss -tulpn | grep :<port>
```

- Find largest files and directories
```sh
du -Sh | sort -rh
```

# Ansible cheatsheet

#### Run one-off commands

```sh
$ ansible [-i <inventory file>] <server group> [-m <module>] \
  [-a <arguments>] [-u <user>] [-B <timeout in seconds>]     \
  [-P <wait time between polling in seconds>]
```


#### Become sudo

```sh
$ ansible [-b] ...
```


## Modules

### Account management

#### Groups (`-m group`)

- Options
  - `name`
  - `state={present,absent}`
  - `gid`
  - `local`
    - 
  - `non_unique`
  - `system`
    - Provides sensible defaults for a "system" group

#### Users (`-m user`)

- Options
  - `name`
  - `group`
  - `createhome`
  - `uid`
  - `shell=[shell]`
  - `password=[encrypted-password]`
    - Set the user's password by supplying an encrypted password.

### Service management

#### Git (`-m git`)

- Options
  - `repo`
  - `dest`
  - `update`
  - `version=[branch, tag, or commit]`

#### Cronjobs (`-m cron`)

- Options
  - `name`
  - `minute`
  - `hour`
  - `day`
  - `month`
  - `weekday`
  - `special_time={yaerly,monthly,reboot}`
  - `job='[command]'`
  - `state={absent}`

### Package management

#### Packages (`-m package`)

### File management

#### File information (`-m stat`)

- Gets file information.
- Options
  - `path=[path]`

#### Copy (`-m copy`)

- Options
  - `src=[source path]`
    - If a trailing slash is provided, only the files _within_ the directory
      are copied. If no trailing slash, then the files _as well as_ the
      directory are copied.
  - `dest=[destination path]`

#### Fetch (`-m fetch`)

- Fetches files from multiple servers, and places them in a folder structure of
  `[dest]/[hostname]/[files]`.

#### File modification (`-m file`)

- Options
  - `state={directory,link,absent}`




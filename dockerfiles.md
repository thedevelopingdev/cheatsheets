# Dockerfile cheatsheet

* `ENTRYPOINT`
  * The default entrypoint is `/bin/sh -c`, with no command.
* `CMD`
  * Specifies arguments that will be fed to the `ENTRYPOINT`.
* `shell` form vs. `exec` form
  * Relevant commands: `ENTRYPOINT`, `CMD`
  * Without the `shlex`-like syntax (i.e. list of commands and arguments), the command is run inside a shell (`shell` form).
    * Running inside a shell entails a process running as `/bin/sh -c <command>`.
  * With the `shlex`-like syntax, the command is not run inside a shell.
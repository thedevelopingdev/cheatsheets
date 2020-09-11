# Linux terminal ricing cheatsheet

- Italics in the terminal

```sh

# A xterm-256color based TERMINFO that adds the escape sequences for italic.
#
# Install:
#
#   tic xterm-256color-italic.terminfo
#
# Usage:
#
#   export TERM=xterm-256color-italic
#
xterm-256color-italic|xterm with 256 colors and italic,
	sitm=\E[3m, ritm=\E[23m,
	use=xterm-256color,

tic xterm-256color-italic
echo "export TERM=xterm-256color-italic" >> ~/.bashrc
```
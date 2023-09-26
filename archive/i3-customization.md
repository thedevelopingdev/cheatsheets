# i3 customization cheatsheet

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

## Wacom tablets

- Sources
	- Manpages: [original](https://www.mankier.com/1/xsetwacom), [archive](https://archive.is/piClb)

```sh
# list all the available wacom devices.
# the device name is in the first column
xsetwacom list

# set Wacom area to specific screen.
# get name of the screen with xrandr
# use HEAD-# instead of HDMI-0, USB-C-0, etc., where #
# is the order the monitor appears in the output of `xrandr`
xsetwacom set "device_name" MapToOutput HEAD-0

# set mode to relative (like a mouse) or absolute (like a pen) mode
xsetwacom set "device_name" Mode "Relative|Absolute"

# rotate the input by 0|90|180|270 degrees from "natural" rotation
xsetwacom set "device_name" Rotate none|half|cw|ccw

# set button to only work when the tip of the pen is touching the tablet
xsetwacom set "device_name" TabletPCButton "on"
```

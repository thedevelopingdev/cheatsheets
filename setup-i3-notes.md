# Installing Minimal Ubuntu with i3 window manager

In accordance to my policy of always taking notes about what I'm doing so that I can remember it later, I've begun writing a series of Markdown files to contain that information.

This file is about the reinstallation of Ubuntu and switching to the i3 tiling window manager after a power outage occured and somehow ruining the configuration on January 21, 2020.

January 21, 2020 was rough day (minimal sleep, things didn't work at work because of insufficient documentation, etc.).



## Remove flashing
There was a problem after installing the NVIDIA drivers where my screen (particular terminal windows with lots of text) would flicker.

* Installed compiz-config-manager --> didn't end up configuring this, so don't believe that it had anything to do with the fix.
* Adding `noveau` to the blacklist worked (add the following file)
*
    ```
    $ cat /etc/modprobe.d/blacklist-nvidia-nouveau.conf

    blacklist nouveau
    options nouveau modeset=0
    ```

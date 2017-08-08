# aws-xubuntu-vnc
setup scripts to configure aws instance as xubuntu desktop supporting remote GUI access over VNC

commands
--------
```
    1  apt update
    2  apt upgrade
    3  apt install xubuntu-desktop
    4  apt install x11vnc
    5  adduser ubuntu-gui
    7  vi /etc/rc.local
    8  vi /usr/local/bin/start-x11vnc
    9  chmod +x /usr/local/bin/start-x11vnc
   10  reboot
   11  cd /root
   13  mkdir .vnc
   14  x11vnc -storepasswd .vnc/passwd
   17  reboot
```

/etc/rc.local
--------
```
#!/bin/sh -e
#
# rc.local
#
# This script is executed at the end of each multiuser runlevel.
# Make sure that the script will "exit 0" on success or any other
# value on error.
#
# In order to enable or disable this script just change the execution
# bits.
#
# By default this script does nothing.

su -l -c '/usr/local/bin/start-x11vnc &'

exit 0
```

/usr/local/bin/start-x11vnc
---------------------------
```
#!/bin/bash
while ! pidof lightdm-gtk-greeter >/dev/null; do sleep 1; done
exec /usr/bin/x11vnc -display :0 -usepw -auth /var/run/lightdm/root/:0 -forever -shared -oa /var/log/x11vnc.log
```


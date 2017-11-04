# DG-M1Q

This is a personal note I want to share.

Some tips to limited the exposition of the cam DG-M1Q.

1- Block the access to the Internet. The default route will be deleted, the cam will not be able to connect to the Internet.
2- Add a user with a password and remove root
3- Disable telnet after 1 minute

# Steps
The first configuration was made with the DigooEye app. Once done, I will not use this app anymore (maybe to check some update) and use another app (like Onvifer on the Play Store).

I retreive the IP address within the app and I connect to it with telnet.

Default login is root without password.

Some of the configuration are store in the /rom folder witch is a persistent folder (eg. modifications inside this folder are saved).

I created a folder _/rom/postconf_
I copied _/etc/passwd_ and _/etc/shadow_ to _/rom/postconf_

I setuped the password for root with the command "_passwd -a 2_" (MD5, we cannot use sha256 or sha256 ... but it's useless because of telnet but better than DES - option _-a 1_).

I added a new user to /rom/postconf/passwd and /rom/postconf/shadow (just copy the root user and rename it).
I copied the password generated on _/etc/shadow_ and copied it to _/rom/postconf/shadow_ for my new user.

I edited _/npc/boot.sh_ (witch is persistent) and added the following lines at the top:
_cp /rom/postconf/passwd /etc/_
_cp /rom/postconf/shadow /etc/_

At this stage, I can connect with my new user with the password.

I change the root password. Now, the access is password protected.

To delete the default gateway, I edited _/npc/dhcp.script_ and add  _/sbin/ip route delete default_ (see the attach file).

There are a better way but it works.

# Reference
* https://github.com/yuvadm/DG-M1Q
* https://github.com/kfowlks/DG-M1Q
* http://adamwesterberg.se/blog/cheap-chinese-camera-teardown

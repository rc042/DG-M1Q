# Intro

This is a personal note I want to share.

Some tips to limited the exposition of the cam DG-M1Q.

I have 2 version:

The old one:
![Old](https://raw.githubusercontent.com/reedcrif/DG-M1Q/master/old_cam.jpg)

The new one
![New](https://raw.githubusercontent.com/reedcrif/DG-M1Q/master/new_cam.jpg)

# DG-M1Q (older version)
1- Block the access to the Internet. The default route will be deleted, the cam will not be able to connect to the Internet.

2- Add a user with a password and remove root

3- Disable telnet after 1 minute

## Steps
The first configuration was made with the DigooEye app. Once done, I will not use this app anymore (maybe to check some update) and use another app (like Onvifer on the Play Store).

Edit: I buy a serial adapter and we must configure the network without the DigooEye app (not tested).

The adapter is https://goo.gl/RXCBHF

Here a quick and dirty connection but works like a charm!

![Quick and dirty connection](https://raw.githubusercontent.com/reedcrif/DG-M1Q/master/20180530_160005.jpg)

I retreive the IP address within the app and I connect to it with telnet (you can do this in a serial console like Putty).

Default login is root without password.

Some of the configuration are store in the /rom folder witch is a persistent folder (eg. modifications inside this folder are saved).

I created a folder _/rom/postconf_

I copied _/etc/passwd_ and _/etc/shadow_ to _/rom/postconf_

I setuped the password for root with the command "_passwd -a 2_" (MD5, we cannot use sha256 or sha256 ... but it's useless because of telnet but better than DES - option _-a 1_).

I added a new user to _/rom/postconf/passwd_ and _/rom/postconf/shadow_ (just copy the root user and rename it).
I copied the password generated on _/etc/shadow_ and copied it to _/rom/postconf/shadow_ for my new user.

I edited _/npc/boot.sh_ (witch is persistent) and added the following lines at the top:

_cp /rom/postconf/passwd /etc/_

_cp /rom/postconf/shadow /etc/_

At this stage, I can connect with my new user with the password.

I change the root password. Now, the access is password protected.

To delete the default gateway, I edited _/npc/dhcp.script_ and add  _/sbin/ip route delete default_ (see the attach file).

There are a better way but it works.

I use a VPN to connect to the stream and an application on my phone:
_rtsp://my-ip:554/onvif1_ (UDP with the credentials I configured)

As a lot of people said, this camera must not be on the Internet for security and privacy reasons.

If you can, you must dedicated a vlan for this kind of devices with strong restrictions.

## Tips
I removed accidentally some files in _/npc_ and I forgot to make a backup. 

Output serial console logs:

```
Media driver version (gcc version 4.6.1 (crosstool-NG 1.18.0) (uClibc)) v1.1.2 #svn r8850 Wed Jul 6 17:44:23 CST 2016
sh: can't open '/npc/boot.sh'
=========================================================================
 Start startup!
startup 0 0
no key detect
vStarNpc:
/npc/upgfile_ok or /npc/npc or /npc/version.txt is not exist!
/bak/npc/npc.tar.gz is not exist!
=========================================================================
```

There is a _/bak_ folder with the _npc.tar.gz_ file.

We need to copy _/bak/npc.tar.gz_ to _/tmp_, then gunzip and tar the archive. Last, move the file to _/npc_ and edit the file as needed.


## Reference
* https://github.com/yuvadm/DG-M1Q
* https://github.com/kfowlks/DG-M1Q
* http://adamwesterberg.se/blog/cheap-chinese-camera-teardown
* https://jumpespjump.blogspot.com/2015/09/how-i-hacked-my-ip-camera-and-found.html?m=1

# DG-M1Q (newer version)
Serial connection is the same as the older cam.

Telnet is activated and credentials are:
```
login: root
password: cxlinux
```

Logs for the 1st boot: https://github.com/reedcrif/DG-M1Q/blob/master/boot.log

Persistent folder is _/home_

```
# mount
rootfs on / type rootfs (rw)
/dev/sys on /sys type sysfs (rw,relatime)
proc on /proc type proc (rw,relatime)
tmpfs on /dev type tmpfs (rw,relatime)
tmpfs on /tmp type tmpfs (rw,relatime)
devpts on /dev/pts type devpts (rw,relatime,mode=600,ptmxmode=000)
/dev/mtdblock2 on /home type jffs2 (rw,relatime)

```

Next step: try to setup the cam without the Digoo-Cloud app. Once the cam is configured, you can remove the default gateway:

```
route delete default gw [IP_DEFAULT_GW]
```

Access to the stream: _rtsp://MY_IP_CAM:554/onvif1_

Login is _admin_ with no password

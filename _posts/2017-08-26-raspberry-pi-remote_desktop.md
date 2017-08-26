---
layout: post
title: Raspberry Pi Remote Desktop using MacBook
---

Here we will see how to access Raspberry Pi's GUI with headless connection using MacBook.

We will use [VNC](https://www.realvnc.com/en/raspberrypi/) for full remote desktop.

Download [VNC Viewer](https://www.realvnc.com/en/connect/download/viewer/) from this page and install it.

Login Raspberry Pi via SSH `ssh pi@raspberrypi.local`, enter password.

Lets configure our Raspberry Pi to allow VNC connection.

So at the command promt in Raspberry Pi, run command `sudo raspi-config`

Select `5 Interfacing Options`

{:.center}
![Raspberry Pi Interfacing Options](/assets/raspberry_pi_interfacing_opts.png)

Select `P3 VNC` to Enable GUI remote access to your Pi using RealVNC

{:.center}
![Raspberry Pi VNC](/assets/raspberry_pi_vnc_opt.png)

Select `Yes` and hit Enter when it asks `Would you like the VNC Server to be enabled?`

{:.center}
![Raspberry Pi VNC](/assets/raspberry_pi_vnc_enable.png)

Now reboot the pi with command `sudo reboot`

Give a few minutes for the Raspberry Pi to fully reboot.

Now lets use VNC Viewer to remote desktop:
* Open the VNC Viewer
* Find the Raspberry Pi's ip address, by running `ping raspberrypi.local`
* Enter that ip address into the VNC Viewer's search box
* Click `Connect to address`
* Enter username: `pi`, password: `raspberry`

It should login and we should see the Raspberry Pi's desktop:

{:.center}
![Raspberry Pi Desktop](/assets/raspberry_pi_desktop.png)

Improve the resolution:

* ssh into pi `ssh pi@raspberrypi.local`
* on pi, run `sudo nano /boot/config.txt`
* add below lines in the HDMI mode section

```
hdmi_ignore_edid=0xa5000080
hdmi_group=2
hdmi_mode=85
```

* reboot pi with `sudo reboot`
* Open VNC viewer to remote desktop and the screen resolution should be much better

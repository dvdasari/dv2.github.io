---
layout: post
title: Raspberry Pi Headless Setup using MacBook
---

Here is how to set up your Raspberry Pi headless, without a keyboard, mouse or monitor.

What we need?
* Raspberry Pi 3 (should work with any Raspberry Pi)
* Micro USB power supply with 2.5A output
* Micro SD Card (16 GB or higher)
* SD Card Adapter
* RJ45 Ethernet cable
* MacBook

{:.center}
![What we need](/assets/raspberry_pi_parts.jpg)

Next, we need to download a few things:
1. Rasbian (Raspberry Pi's Operating System)

    Go to [Raspberry Pi Downloads](https://www.raspberrypi.org/downloads/) and click on RASPBIAN and Download ZIP file. This file is about 1.6 GB, so will take a little while to download. This is a zip file so we will need The Unarchiver to unzip this.

2. The Unarchiver (to unzip files)

    Go to [The Unarchiver](https://theunarchiver.com/) and either download directly from their site or from the App Store.

3. Pi FILLER (makes it super easy to copy the Raspberry Pi Operating System to the SD Card)

    Go to [Pi Filler](http://ivanx.com/raspberrypi/) and download Pi Filler

Now that the required downloads are done.

1. Extract the Raspbian zip file with The Unarchiver and you should see an img file after extraction.
2. Extract the PiFiller zip file with The Unarchiver.

Now open Pi Filler and follow the instructions.

* Navigate to the img file folder, choose the image file and click Choose.
* It should prompt to insert the SD card.
* Insert SD Card adapter (with the micro SD card inserted). Hit Continue on the Prompt for Pi Filler.
* It searches for SD Card. Hit Continue after it finds.
* It prompts about erasing SD Card and copying Raspberry Pi os. Hit Erase SD Card, enter mac password.
* It will give an estimated time remaining, wait until it is done. It will prompt when its is done.
* When it is done, hit Quit and remove the SD Card.

SSH (Secure Shell) access to your Raspberry Pi.

* For headless setup, SSH can be enabled by placing a file named `ssh`, without any extension, onto the boot partition of the SD card. When the Pi boots, it looks for the `ssh` file. If it is found, SSH is enable, and the file is deleted. The content of the file does not matter.
* Insert the SD card back into your MacBook.
* You should see a `boot` drive.
* Create a file named `ssh` (without any extension) and copy it to this drive.
* Now eject the card
* Take micro SD card out of the SD Card adapter
* Plug the micro SD card into the Raspberry Pi
* Plug the ethernet cable into the Pi
* Plug the power cord into the Pi
* Wait a few minutes for the Pi to fully power up
* Open Terminal / iTerm or your mac and run the command `ping raspberrypi.local`
* that should show the pi's ip address
* exit from the ping
* now run the command `ssh pi@ip-address` (where ip-address is the ip address from the ping output)
* enter `yes` when it asks `Are you sure you want to continue connecting`
* enter password: `raspberry` (this is the default password)
* it should ssh into you pi and you will see the promt `pi@raspberrypi:~ $`

# Yeahhhh you are now ssh'ed into your Pi...

If you want to change configuration, type `sudo raspi-config` and this will display a GUI with options.

{:.center}
![Raspberry Pi Config](/assets/raspberry_pi_config.png)

If you want to make all of the SD card storage available to the OS

* Select `7 Advanced Options`, Hit enter

{:.center}
![Raspberry Pi Advanced Options](/assets/raspberry_pi_adv_opts.png)

* Select `A1 Expand Filesystem` Hit enter

{:.center}
![Raspberry Pi Expand Fs](/assets/raspberry_pi_expand_fs.png)

And then reboot the pi to make these changes effective.

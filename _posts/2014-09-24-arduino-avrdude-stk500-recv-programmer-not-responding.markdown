---
layout: post
title:  Problem uploading to Arduino board — avrdude: stk500_recv(): programmer is not responding
date:   2014-09-24 19:35:03 -0500
comments: true
keywords: Arduino avrdude stk500
description:  Problem uploading to Arduino board — avrdude: stk500_recv(): programmer is not responding
---

Model: Arduino Uno R3

A new Arduino comes with a pre-programmed LED blink program. When the Arduino is plugged into your Mac via USB, the LED should flash on and off once a second.

I was very excited to write some code and run it on the Arduino. I started with a change to the blink program and tried to upload. I got an error message at the bottom of the IDE.

```
Binary sketch size: 1,082 bytes (of a 32,256 byte maximum)
avrdude: stk500_recv(): programmer is not responding
```

Screenshot:

![Error Screenshot]({{ site.url }}/assets/arduino_error_screenshot.png)

I did the usual "google the error message". And read posts where I found that, resetting the Uno and reloading the program would work. So I did as suggested, but got the same error again.

Then I went to the IDE Tool bar and tried to switch to different Serial Port and tried uploading the program. Finally selecting `/dev/tty.usbmodem1411` did the trick.

It worked!!!!

![IDE Toolbar]({{ site.url }}/assets/arduino_ide_toolbar.png)

Hopefully this post will save time for someone with the same error.

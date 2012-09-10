# adafruit-raspberrypi-linux-pps

Kernel image and modules with PPS-GPIO added. This will allow you to use PPS with NTP on a Raspberry Pi.

Please see [this page](https://github.com/davidk/adafruit-raspberrypi-linux), branch: ppsgpio for the source this is based on.

## Installation

To install this kernel, copy kernel.img to boot:

```bash
$ sudo su
# mv /boot/kernel.img /boot/kernel.img.orig
# mv kernel.img /boot/
```

Copy the modules:

```bash
# mv modules/* /lib/modules
```

Add the `pps-gpio` module to `/etc/modules`:

```bash
# echo "pps-gpio" >> /etc/modules
```

Reboot:

```bash
# reboot
```

Once your system is back up, check to see that the kernel version has changed and that pps-gpio loaded:

```bash
$ uname -a
Linux raspberrypi 3.1.9adafruit-pps+ #21 PREEMPT Sun Sep 2 10:57:58 PDT 2012 armv6l GNU/Linux
$ dmesg | grep pps
.. snip ..
[   10.339448] pps pps0: new PPS source pps-gpio.-1
[   10.341591] pps pps0: Registered IRQ 108 as PPS source
```

PPS input will be available on pin 23 of the Raspberry Pi header.

## Credits
All of these modifications were cobbled together by reading this [thread and following its instructions](http://www.raspberrypi.org/phpBB3/viewtopic.php?f=9&t=1970)

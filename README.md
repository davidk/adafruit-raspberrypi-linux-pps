# adafruit-raspberrypi-linux-pps

Kernel image and modules with PPS-GPIO added. This will allow you to use PPS with NTP.

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
# sudo reboot
```

Once your system is back up, check to see that the kernel version has changed and that pps-gpio loaded:

```bash
$ uname -a
Linux raspberrypi 3.1.9adafruit-pps+ #21 PREEMPT Sun Sep 2 10:57:58 PDT 2012 armv6l GNU/Linux
$ dmesg | grep pps
[    0.000000] Linux version 3.1.9adafruit-pps+ (davidk@bender) (gcc version 4.6.3 (Ubuntu/Linaro 4.6.3-1ubuntu5) ) #21 PREEMPT Sun Sep 2 10:57:58 PDT 2012
[    1.154697] usb usb1: Manufacturer: Linux 3.1.9adafruit-pps+ dwc_otg_hcd
[   10.319818] pps_core: LinuxPPS API ver. 1 registered
[   10.321766] pps_core: Software ver. 5.3.6 - Copyright 2005-2007 Rodolfo Giometti <giometti@linux.it>
[   10.339448] pps pps0: new PPS source pps-gpio.-1
[   10.341591] pps pps0: Registered IRQ 108 as PPS source
```

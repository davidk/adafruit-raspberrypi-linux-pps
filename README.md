# adafruit-raspberrypi-linux-pps

Kernel image and modules with PPS-GPIO added. This will allow you to use PPS with NTP on a Raspberry Pi. 

This repository is used as a part of this [article](http://open.konspyre.org/blog/2012/10/18/raspberry-pi-time-server/), so check
that out for help.

For source modifications, please see [this page](https://github.com/davidk/adafruit-raspberrypi-linux), branch: ppsgpio for the source 
this is based on.

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
[ .. snip .. ]
[   10.339448] pps pps0: new PPS source pps-gpio.-1
[   10.341591] pps pps0: Registered IRQ 108 as PPS source
```

PPS input will be available on pin 23 of the Raspberry Pi header. 

**Note**: Ensure that your GPS unit's PPS output is 3.3v friendly! 

If you're looking for a unit which fulfills this requirement, Adafruit's Ultimate GPS (all versions) 
has a 1PPS output which is around ~2.8V, and is compatible with the Raspberry Pi.

Once connected, install `pps-tools` to test:

```bash
$ sudo aptitude install pps-tools
[ .. snip .. ]
Unpacking pps-tools (from .../pps-tools_0.20120406+g0deb9c7e-2_armhf.deb) ...
Processing triggers for man-db ...
Setting up pps-tools (0.20120406+g0deb9c7e-2) ..
```
Assuming that the pps line is on /dev/pps0:

```bash
$ sudo ppstest /dev/pps0
trying PPS source "/dev/pps0"
found PPS source "/dev/pps0"
ok, found 1 source(s), now start fetching data...
source 0 - assert [....], sequence: [....] - clear  [....], sequence: [....]
```

If your output looks something like the above (brackets are numbers removed for clarity), you're all set. If not, 
verify that everything is connected and that the pps-gpio kernel module is loaded (along with the -pps kernel itself).

It may be helpful to scope the PPS line to see if the GPS module is actually sending a signal. On some units, the 1PPS
output only becomes active after a fix.

## Credits
All of these modifications were cobbled together by reading this [thread and following its instructions](http://www.raspberrypi.org/phpBB3/viewtopic.php?f=9&t=1970).

See [this repository](https://github.com/davidk/adafruit-raspberrypi-linux), (branch: ppsgpio) for source code.
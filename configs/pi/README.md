# Raspberry Pi

These configurations target the Raspberry Pi family of boards.

## Board Compatibility

There are specific packages tuned to each Pi model. However, certain packages
are forwards or backwards compatible.

| **Board**       | **Config Package** |
| --------------- | -----------------  |
| [Pi 0]          | pi/0               |
| [Pi 1]          | pi/1 or pi/3       |
| [Pi 2]          | pi/3               |
| [Pi 3]          | pi/3 or pi/4       |
| [Pi 4]          | pi/4 or pi/3       |
| [Pi 4] - 64 bit | pi/4x64            |

[Pi 4]: https://www.raspberrypi.org/products/raspberry-pi-4-model-b/
[Pi 3]: https://www.raspberrypi.org/products/raspberry-pi-3-model-b/
[Pi 2]: https://www.raspberrypi.org/products/raspberry-pi-2-model-b/
[Pi 1]: https://www.raspberrypi.org/products/raspberry-pi-1-model-b/
[Pi 0]: https://www.raspberrypi.org/products/raspberry-pi-zero/

## Note: config.txt

You can override the config.txt. Simply copy the "resources/rpi" directory from
the pi/common configuration into your own configuration package, for example:
"mypackage/resources/rpi/config.txt"

## Note: dtoverlay config.txt

Upstream adds `dtoverlay=miniuart-bt` to the config.txt, which should "fix
ttyAMA0 serial console.

## Note: 64 bit kernel

The pi/4x64 configuration uses a 64 bit kernel with an alternate config.txt,
which specifies `arm_64bit` as required.

Raspbian does not use 64 bit yet and many of the video drivers don't work with
aarch64 yet.

According to the Gentoo wiki:

  The Raspberry Pi closed source VC4 driver is not available on 64-bit
  (ARM64/AARCH64) systems. The Raspberry Pi foundation has stated "we are not
  working on this, and are unlikely to do so in the near future". Using the open
  source vc4-fkms-v3d driver listed below instead is recommended.

References:

 - https://wiki.gentoo.org/wiki/Raspberry_Pi_VC4
 - https://github.com/raspberrypi/linux/issues/2315#issuecomment-383132350

## Overclocking

You will want to follow the upstream guidance on overclocking: 

https://www.raspberrypi.org/documentation/configuration/config-txt/overclocking.md

Add the following snippet at the end of config.txt for a quick start. WARNING!
This will set a permanent bit on your Pi which will mark it as having been
overclocked. This will most-likely void the warranty.

```
over_voltage=4
force_turbo=1
max_usb_current=1
```


## TODO config sections and universal config package

The config.txt file supports conditional sections:

 - `[pi1]`:	Model A, Model B, Compute Module
 - `[pi2]`:	Model 2B (BCM2836- or BCM2837-based)
 - `[pi3]`:	Model 3B, Model 3B+, Model 3A+, Compute Module 3
 - `[pi3+]`:	Model 3A+, Model 3B+
 - `[pi4]`:	Model 4B
 - `[pi0]`:	Zero, Zero W, Zero WH
 - `[pi0w]`:	Zero W, Zero WH

Skiff could have a "universal" pi/all configuration which targets all major
boards with conditional loading of the uimage depending on 32bit or 64bit OS.


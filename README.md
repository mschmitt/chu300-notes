# Notes on the Chuwi Minibook X U300

***

## ATTENTION

**The USB-C charger shipping with the Chuwi book is a non-compliant non-negotiating fixed 12 V power supply.**

**It will KILL regular USB-C devices and DID so with two USB gadgets of mine!**

The notebook charges fine on compliant USB-C chargers from Apple and Lenovo. No idea what the hell is wrong with people.

![Screenshot from a game: "Clipboard of grudges - gain XP when you take damage"](images/vlcsnap-2026-09-21-09h59m55s417.png)

***

https://de.chuwi.com/products/minibook-x-u300

This is a beautifully fast little machine with a just slightly cramped keyboard. However, it's excellent display is a rotated tablet display, which can make it quite daunting to set up.

 * Note that you can always use an external monitor to work around and on the display's limitations.
 * Initial installation using an external monitor probably is the path of least resistance.

I installed Cachy OS with the following key components:

 * KDE Plasma
 * Plasma Login Manager
 * Limine bootloader
 * Splash / Plymouth active

Thanks to the rotation capability of the bootloader, this installation *never* exposes a rotated screen.

All issues occurred identically with Debian 13 "Trixie".

## GPU driver notes

* GPU initialization on bootup seems highly unstable, showing a `DSI link not ready` error in the output of `dmesg`.
* When this happens (it does *most* of the time) and the device shows only weird artifacts after booting, **DO NOT PANIC**, but suspend and resume.
* I have no fix or reproducer for the subtle display flicker that I've seen several times. There *is* a possibility that there's a correlation with use of the laptop's own inferior power supply.
* I *tried* to somehow nudge the *i915* driver behaviour by delaying its loading in */etc/modprobe.d/i915.conf*, which *seems* to work *some* of the time:

```
install i915 /usr/bin/sleep 5; /usr/bin/modprobe --ignore-install i915
```

With this workaround in place, the *xe* driver seems to try to take precedece over *i915*, so it needs to be blocklisted in the kernel command line in */etc/default/limine*:

```
module_blacklist=xe
```

## Display rotation notes

### Firmware

Screen rotation in firmware setup:

 * Right rotation.
 * Quiet boot disabled.
 * No further changes other than Secureboot disabled.

### Bootloader / Kernel

Screen rotation in */boot/limine.conf*:
    
```
interface_rotation: 90
```

Kernel command line in */etc/default/limine* extended:

```
video=DSI-1:panel_orientation=right_side_up 
```

### Desktop

 * The display was rotated in KDE display settings to match the laptop's orientation. 
 * Then the plasma display settings were copied to the login screen settings using the GUI.
 * Never used in tablet or tent mode.

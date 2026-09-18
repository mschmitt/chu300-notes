# Notes on the Chuwi Minibook X U300

https://de.chuwi.com/products/minibook-x-u300

This is a beautifully fast and not at all toylike machine. However, it's excellent display is a rotated tablet display, which can make it quite daunting to set up.

 * Note that you can always use an external monitor to work around and on the display's limitations.
 * Initial installation using an external monitor probably is the path of least resistance.

## GPU driver notes

* GPU initialization on bootup seems highly unstable, showing a `DSI link not ready` error in the output of `dmesg`.
* When this happens (it mostly does) and the device shows only weird artifacts after booting, DO NOT PANIC, but suspend and resume.
* I have no fix for the subtle display flicker that sometimes occurs, other than to reduce brightness below 20%.
Preppern
I *tried* to somehow nudge the *i915* driver behaviour by delaying its loading in */etc/modprobe.d/i915.conf*, which *seems* to work *some* of the time:

```
install i915 /usr/bin/sleep 5; /usr/bin/modprobe --ignore-install i915
```

With this workaround in place, the *xe* driver seems to try to take precedece over i915 sometimes, so it needs to be blocklisted in the kernel command line:

```
module_blacklist=xe
```

## Display rotation notes

I run Cachy OS with the following key components:

 * KDE Plasma
 * Plasma Login Manager
 * Limine bootloader

Thanks to the rotation capability of the bootloader, this installation *never* shows a rotated screen.

### Desktop

The display was rotated in KDE display settings to match the laptop's orientation. (Never used in tablet or "tent" mode.) Then the plasma display settings were copied login screen settings using the GUI.

### Bootloader / Kernel

Screen rotation in */boot/limine.conf*:
    
```
interface_rotation: 90
```

*cmdline* in */etc/default/limine* extended:

```
video=DSI-1:panel_orientation=right_side_up 
```
### Firmware

Screen rotation in firmware setup:

 * Right rotation
 * No further changes other than Secureboot disabled

## Other distributions

All issues occurred identically with Debian 13 "Trixie".

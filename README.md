# Notes on the Chuwi Minibook X U300

https://de.chuwi.com/products/minibook-x-u300

This is a beautifully fast and not at all toylike machine. However, it's excellent display is a rotated tablet display, which can make it quite daunting to set up.

 * Note that you can always use an external monitor to work around and on the display's limitations.

## GPU driver notes

* GPU initialization on bootup seems highly unstable, showing a `DSI link not ready` error in the output of `dmesg`.
* When this has happened (it mostly does) and the device shows weird artifacts after booting, suspend and resume.
* I have no fix for the subtle display flicker that sometimes occurs, other than reducing brightness below 20%.

I *tried* to somehow nudge the *i915* driver behaviour by delaying its loading in */etc/modprobe.d/i915.conf*, which *seems* to work *some* of the time:

```
install i915 /usr/bin/sleep 5; /usr/bin/modprobe --ignore-install i915
```

With this workaround in place, the *xe* driver seems to try to take precedece over i915 sometimes, so it needs to be blocklisted in the kernel command line:

```

I run Cachy OS with KDE Plasma and plasma-login-manager. Display rotated in KDE Display settings to match the laptop's orientation. (Never used in tablet or "tent" mode.) Then copied Plasma settings to login screen settings using the GUI.

Screen rotation in firmware setup:

 * Right rotation
 * No other changes other than Secureboot disabled

Screen rotation in */boot/limine.conf*:
    
```
interface_rotation: 90
```

*cmdline* in */etc/default/limine* extended:

```
video=DSI-1:panel_orientation=right_side_up module_blacklist=xe
```

The xe driver seems to be in an eternal conflict with the i915 driver if loading of i915 is delayed as follows. This hack in */etc/modprobe.d/i915.conf* seems to fix the artifacts some of the time:
    
```
install i915 /usr/bin/sleep 5; /usr/bin/modprobe --ignore-install i915
```













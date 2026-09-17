# Chuwi Minibook X U300

The device you scanned this QR code off of is a CHUWI Minibook X U300:
    
https://de.chuwi.com/products/minibook-x-u300

This is a beautifully fast and not at all toylike machine, which can be a bit daunting to set up.

First of all, it uses a rotated tablet screen, which has excellent display quality but is a bit of a challenge.

* You can always use an external monitor to work around and on the display's limitations.
* If your device shows weird artifacts after booting, suspend and resume.
* I have no fix for the subtle display flicker that sometimes occurs.

I run Cachy OS with KDE Plasma and plasma-login-manager. Display rotated in KDE Display settings to match the laptop's orientation. (Never used in tablet or "tent" mode.) Then copied Plasma settings to login screen settings using the GUI.

Screen rotation in */boot/limine.conf*:
    
```
interface_rotation: 90
```

*cmdline* in */etc/limine.conf* extended:

```
video=DSI-1:panel_orientation=right_side_up module_blacklist=xe
```

The xe driver seems to be in an eternal conflict with the i915 driver if loading of i915 is delayed as follows. This hack in */etc/modprobe.d/i915.conf* seems to fix the artifacts:
    
```
install i915 /usr/bin/sleep 5; /usr/bin/modprobe --ignore-install i915
```













# Framework Laptop 16 Files

This repository contains files that fix issues or modify behavior of the Framework Laptop 16 when using Linux.

## QMK/VIA Keyboard: Left CTRL and Fn Swap

In order to swap the left `CTRL` and `Fn` keys using Framework's version of [VIA](https://keyboard.frame.work/), a `udev` rule needs to be added in order for the application to access and modify the QMK firmware.

Copy the `50-qmk.rules` file under `etc/udev/rules.d` to `/etc/udev/rules.d` and run the following commands:

```bash
sudo udevadm control --reload-rules
sudo udevadm trigger
```

Located under `qmk-via` is the exported JSON file that includes the mappings to swap the two keys both for layers 0 and 2.

## Disable Wireless Power Saving

Wireless network power saving can be disabled on a per-connection basis using the `nmcli` utility:

```bash
nmcli connection modify <ssid> wifi.powersave 2
```

To revert to the default value, run:

```bash
nmcli connection modify <ssid> wifi.powersave 0
```

## 'License'

Files and documentation included in this repository is free and unemcumbered software released into the public domain. For more information, see the included UNLICENSE file.

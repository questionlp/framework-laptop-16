# Framework Laptop 16 Files

This repository contains files that fix issues or modify behavior of the Framework Laptop 16 when using Linux.

## Using OpenH264 and Mesa from RPM Fusion

See [Installing OpenH264 and Mesa from RPM Fusion](./installing-openh264-and-mesa-from-rpmfusion.md)

## Fix Headset Microphone Input

There is a known issue with the default Fedora install may not correctly handling headset microphone input from the audio expansion card.

```bash
sudo tee /etc/modprobe.d/alsa-base.conf <<< "echo options snd-hda-intel index=1,0 model=auto,dell-headset-multi"
```

Source: <https://community.frame.work/t/solved-headset-mic-on-amd-fw13-running-fedora-39/38847/2>

## QMK/VIA Keyboard: Left CTRL and Fn Swap and Right CTRL and ALT Swap

In order to swap the left `CTRL` and `Fn` keys (and, optionally, swapping the right `CTRL` and `ALT` keys) using Framework's version of [VIA](https://keyboard.frame.work/), a `udev` rule needs to be added in order for the application to access and modify the QMK firmware.

Copy the `50-qmk.rules` file under `etc/udev/rules.d` to `/etc/udev/rules.d` and run the following commands:

```bash
sudo udevadm control --reload-rules
sudo udevadm trigger
```

I also mapped `Fn` + `Tab` to `Scroll Lock` in order to use `Scroll Lock` as a `Compose` key in KDE.

Located under `qmk-via` are exported JSON files containing the various swaps and mappings:

| Keymap JSON File | Description |
| --- | --- |
| [fl16-ansi-swaps-lctrl_fn-ralt_rctl-scrolllock-map.json](qmk-via/fl16-ansi-swaps-lctrl_fn-ralt_rctl-scrolllock-map.json) | Swapped: Left Ctrl and Fn, Swapped: Right Alt and Right Ctrl, Mapped: Fn+Tab to Scroll Lock |
| [fl16-ansi-swaps-lctrl_fn-ralt_rctl.json](qmk-via/fl16-ansi-swaps-lctrl_fn-ralt_rctl.json) | Swapped: Left Ctrl and Fn, Swapped: Right Alt and Right Ctrl |
| [fl16-ansi-swaps-lctrl_fn.json](qmk-via/fl16-ansi-swaps-lctrl_fn.json) | Swapped: Left Ctrl and Fn |

## Disable Wireless Power Saving

Wireless network power saving can be disabled on a per-connection basis using the `nmcli` utility:

```bash
nmcli connection modify <ssid> wifi.powersave 2
```

To revert to the default value, run:

```bash
nmcli connection modify <ssid> wifi.powersave 0
```

## Improving Laptop Sound

In many cases, the sound that comes out of the speakers sounds a bit muddy and does not have good sound staging. To improve things a bit, I use [EasyEffects](https://github.com/wwmm/easyeffects) and the "Laptop" preset provided by [JackHack96/EasyEffects-Presets](https://www.youtube.com/watch?v=liQjYpy7KeE).

## 'License'

Files and documentation included in this repository is free and unemcumbered software released into the public domain. For more information, see the included UNLICENSE file.

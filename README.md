# Framework Laptop 16

This repository contains files that fix issues or modify behavior of the Framework Laptop 16 when using Linux.

## Fedora: Package Installation

*This only applies to non-immutable versions and spins of Fedora.*

### Prepping Fedora for Web Development

See [Prepping Fedora for Python, Ruby and Node.js Web Development](./prepping-fedora-for-web-development.md)

### Using OpenH264 and Mesa from RPM Fusion

See [Installing OpenH264 and Mesa from RPM Fusion](./installing-openh264-and-mesa-from-rpmfusion.md)

## power-profiles-daemon, Power Save and Reduced Screen Contrast

When setting the Power Profile of the Framework Laptop 16 to "Power Save" mode and running on battery, power-profiles-daemon and the AMD GPU driver will put the display panel in low power mode, thus causing the display have lower contrast.

I have created an [open issue](https://github.com/questionlp/framework-laptop-16/issues/1) in this repository to provide a temporary workaround until a more permanent solution has been implemented.

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

I also mapped `Fn` + `Caps Lock` to `Scroll Lock` in order to use `Scroll Lock` as a `Compose` key. There is a variation of the map to use `Fn` + `Tab` instead of `Fn` + `Caps Lock`.

Located under `qmk-via` are exported JSON files containing the various swaps and mappings:

| Keymap JSON File | Description |
| --- | --- |
| [fl16-ansi-swaps-lctrl_fn-ralt_rctl-fnmap-capslock_scrolllock.json](qmk-via/fl16-ansi-swaps-lctrl_fn-ralt_rctl-fnmap-capslock_scrolllock.json) | Swapped: Left `Ctrl` and `Fn`, Swapped: Right `Alt` and Right `Ctrl`, Mapped: `Fn` + `Caps Lock` to Scroll Lock |
| [fl16-ansi-swaps-lctrl_fn-ralt_rctl-fnmap-tab_scrolllock.json](qmk-via/fl16-ansi-swaps-lctrl_fn-ralt_rctl-fnmap-tab_scrolllock.json) | Swapped: Left `Ctrl` and `Fn`, Swapped: Right `Alt` and Right `Ctrl`, Mapped: `Fn` + `Tab` to Scroll Lock |
| [fl16-ansi-swaps-lctrl_fn-ralt_rctl.json](qmk-via/fl16-ansi-swaps-lctrl_fn-ralt_rctl.json) | Swapped: Left `Ctrl` and `Fn`, Swapped: Right `Alt` and Right `Ctrl` |
| [fl16-ansi-swaps-lctrl_fn.json](qmk-via/fl16-ansi-swaps-lctrl_fn.json) | Swapped: Left `Ctrl` and `Fn` |

### Mapping `Scroll Lock` to `Compose`

The steps provided have been tested in Fedora 40 (KDE Plasma 6) and Debian 12 (GNOME, Cinnamon, MATE and XFCE).

#### KDE Plasma 6

1. In the "System Settings" application, browse to "Keyboard" under "Input & Output"
2. Click on the "Key Bindings" tab
3. Scroll down to and expand the "Position of Compose Key" item
4. Check "Scroll Lock"
5. Click "Apply" to save the settings

#### GNOME

1. In the "Settings" application, browse to "Keyboard"
2. Under "Special Character Entry", click on "Compose Key"
3. Click on the toggle to enable the "Compose Key" option
4. Select "Scroll Lock" from the list and close the dialog

#### Cinnamon

1. Click on the Cinnamon Menu button, browse to "Keyboard" under "Preferences"
2. In the "Keyboard" settings window, click on the "Layouts" tab
3. In the lower portion of the window, click on "Options..."
4. In the "Keyboard Layout Options" dialog, scroll down to and expand the "Position of Compose Key" item
5. Check "Scroll Lock"
6. Click on the "Close" button to close the dialog

#### MATE

1. Click on the "System" menu and browse to "Keyboard" under "Preferences" and "Hardware"
2. In the "Keyboard Preferences" window, click on the "Layout" tab
3. In the lower portion of the window, click on the "Options..."
4. In the "Keyboard Layout Options" dialog, scroll down to and expand the "Position of Compose Key" item
5. Check "Scroll Lock"
6. Click on the "Close" button to close the dialog

#### XFCE

1. Click on the "Applications" menu and browse to "Keyboard" under "Settings"
2. In the "Keyboard" settings window, click on the "Layout" tab
3. Click on the toggle to disable the "Use system defaults" option
4. In the "Compose key" dropdown, select "Scroll Lock"
5. Click on the "Close" button to close the settings window

## Disable Wireless Power Saving

*This may not be necessary for all cases, but including it for reference.*

Wireless network power saving can be disabled on a per-connection basis using the `nmcli` utility:

```bash
nmcli connection modify <ssid> wifi.powersave 2
```

To revert to the default value, run:

```bash
nmcli connection modify <ssid> wifi.powersave 0
```

## Improving Laptop Sound

In many cases, the sound that comes out of the speakers sounds a bit muddy and does not have good sound staging. To improve things a bit, I use [EasyEffects](https://github.com/wwmm/easyeffects) and the "Laptop" preset provided by [JackHack96/EasyEffects-Presets](https://github.com/JackHack96/EasyEffects-Presets).

## 'License'

Files and documentation included in this repository is free and unemcumbered software released into the public domain. For more information, see the included UNLICENSE file.

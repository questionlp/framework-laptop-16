# Framework Laptop 16

This repository contains files that fix issues or modify behavior of the Framework Laptop 16 when using Linux.

## Fedora: Package Installation

*This only applies to non-immutable versions and spins of Fedora.*

### Prepping Fedora for Web Development

See [Prepping Fedora for Python, Ruby and Node.js Web Development](./prepping-fedora-for-web-development.md)

### Using OpenH264 and Mesa from RPM Fusion

See [Installing OpenH264 and Mesa from RPM Fusion](./installing-openh264-and-mesa-from-rpmfusion.md)

## Disabling Fedora Flatpak Remotes

Due to issues with Flatpak applications that are distributed via Fedora's Flatpak repo not being official versions, up to date, or have other issues, it is recommended to disable both `fedora` and `fedora-testing` remotes and stick with installing Flatpaks from [Flathub](https://flathub.org/) instead.

To disable either of the Fedora remotes, run the following command for the specific remote to disable:

```bash
flatpak remote-modify --no-filter --disable fedora
flatpak remote-modify --no-filter --disable fedora-testing
```

To re-enable either of the remotes, run the following command:

```bash
flatpak remote-modify --no-filter --enable fedora
flatpak remote-modify --no-filter --enable fedora-testing
```

To ensure that the Flathub remote is available, run the following command (provided by [Flathub](https://flathub.org/setup/Fedora)):

```bash
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

## power-profiles-daemon, Power Save and Reduced Screen Contrast

When setting the Power Profile of the Framework Laptop 16 to "Power Save" mode and running on battery, power-profiles-daemon and the AMD GPU driver will put the display panel in low power mode, thus causing the display have lower contrast.

A solution has been found in the README.md file for [power-profiles-daemon](https://gitlab.freedesktop.org/upower/power-profiles-daemon) git repo. I have posted more information in [issue #1](https://github.com/questionlp/framework-laptop-16/issues/1).

With Fedora 41 switching from using `power-profiles-daemon` to `tuned-ppd`, the method of automatically setting the display panel power mode needs to be updated. Steps are provided in [issue #2](https://github.com/questionlp/framework-laptop-16/issues/2). Corresponding files have been added to the repository under `etc/tuned`.

## Fedora 41: Replacing `tuned` with `power-profiles-daemon`

To replace the default `tuned` with the Framework recommended `power-profiles-daemon`, you can run the following on a fully updated system:

```bash
sudo dnf remove tuned
sudo dnf install power-profiles-daemon
```

Once `power-profiles-daemon` has been installed, you will want to restart your laptop as soon as possible for the changes to fully take effect.

SELinux may report a violation when switching power profiles. To disable the warning, you can run the following as **root**:

```bash
ausearch -c 'power-profiles-' --raw | audit2allow -M my-powerprofiles
semodule -X 300 -i my-powerprofiles.pp
```

You may need to [re-apply the reduced screen contrast fix](https://github.com/questionlp/framework-laptop-16/issues/1) for `power-profiles-daemon` mentioned above if you made the changes in Fedora 40, upgraded to Fedora 41, then replace `tuned` with `power-profiles-daemon`.

## Fedora 41: AMD GPU Screen Glitches and Corruption

See [issue #3](https://github.com/questionlp/framework-laptop-16/issues/3) for steps to add `amdgpu.sg_display=0` to the kernel arguments using `grubby` as a potential fix for random screen glitches and corruption that can occur with AMD GPUs.

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
| [fl16-rgb-ansi-fnmap-capslock_scrolllock.json](qmk-via/fl16-rgb-ansi-fnmap-capslock_scrolllock.json) | RGB US English keyboard - Mapped: `Fn` + `Caps Lock` to Scroll Lock |
| [fl16-rgb-ansi-swaps-ralt_rctl-fnmap-capslock_scrolllock.json](qmk-via/fl16-rgb-ansi-swaps-ralt_rctl-fnmap-capslock_scrolllock.json) | RGB US English keyboard - Swapped: Right `Ctrl` and Right `Alt`, Mapped: `Fn` + `Caps Lock` to Scroll Lock |

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
nmcli connection modify ssid 802-11-wireless.powersave 2
```

To revert to the default value, run:

```bash
nmcli connection modify ssid 802-11-wireless.powersave 0
```

## Improving Laptop Sound

In many cases, the sound that comes out of the speakers sounds a bit muddy and does not have good sound staging. To improve things a bit, I use [EasyEffects](https://github.com/wwmm/easyeffects) and the "Laptop" preset provided by [JackHack96/EasyEffects-Presets](https://github.com/JackHack96/EasyEffects-Presets).

## VS Code: Inserting of indents when using KDE Task Switcher

While this is not specific to the Framework Laptop 16 or even Fedora, but it is an annoyance when using KDE Task Switcher via `Alt` + `Tab`. A workaround is to set "Allow legacy X11 apps to read keystrokes typed in all apps" to "Never" instead of the default "As above, plus any key typed while the Control, Alt, or Meta keys are pressed".

Keep in mind that this might break functionality of other legacy X11 applications outside of VS Code.

## 'License'

Files and documentation included in this repository is free and unemcumbered software released into the public domain. For more information, see the included UNLICENSE file.

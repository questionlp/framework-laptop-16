# Installing OpenH264 and Mesa from RPM Fusion

The majority of the instructions come or have been adapted from the RPM Fusion [Multimedia wiki page](https://rpmfusion.org/Howto/Multimedia).

## Enabling and Installing Cisco OpenH264

```bash
sudo dnf config-manager --set-enabled fedora-cisco-openh264
sudo dnf install gstreamer1-plugin-openh264 mozilla-openh264
```

## Setting Up RPM Fusion Repositories

```bash
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

## Install Full ffmpeg

```bash
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
```

## Installing Mesa Drivers for AMD Radeon

```bash
sudo dnf swap mesa-va-drivers mesa-va-drivers-freeworld
sudo dnf swap mesa-vdpau-drivers mesa-vdpau-drivers-freeworld
```

## Installing Multimedia Programs, Tools and Dependencies from RPM Fusion

```bash
sudo dnf install libva-utils mpv
sudo dnf update @multimedia --setopt="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin
sudo dnf update @sound-and-video
```

## Install Support for Playing DVDs

```bash
sudo dnf install rpmfusion-free-release-tainted
sudo dnf install libdvdcss
```

## Install Additional Firmware

```bash
sudo dnf install rpmfusion-nonfree-release-tainted
sudo dnf --repo=rpmfusion-nonfree-tainted install "*-firmware"
```

## Enable VA-API and OpenH264 in Firefox

Open Firefox, browse to `about:config` and set the following parameters to `true` (add them if they are not already present):

* media.gmp-gmpopenh264.autoupdate
* media.gmp-gmpopenh264.enabled
* media.gmp-gmpopenh264.provider.enabled
* media.peerconnection.video.h264_enabled
* media.ffmpeg.vaapi.enabled

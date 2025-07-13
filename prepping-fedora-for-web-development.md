# Prepping Fedora for Python, Ruby and Node.js Web Development

## Installing First Set of Packages

These packages include some basic utilities and fonts that I use regularly.

```bash
sudo dnf install htop tmux vim fastfetch git gh zsh avahi-tools
```

### Optional: Install Development Fonts

```bash
sudo dnf install mozilla-fira-mono-fonts mozilla-fira-sans-fonts fira-code-fonts jetbrains-mono-fonts-all cascadia-fonts-all cascadia-code-fonts cascadia-code-pl-fonts ibm-plex-fonts-all
```

## Fedora KDE Plasma Spin: Removing MariaDB

Since I prefer to install MySQL Community Server from the Oracle MySQL Yum repository, I first need to remove MariaDB, which is installed as a dependency for several KDE applications. Removing MariaDB will break Akonadi, Kontact and KOrganizer. That said, I don't plan on using those applications.

```bash
sudo dnf remove mariadb
```

## Fedora Workstation 42: Installing MySQL from Oracle Yum Repository

Fedora Workstation 42 includes Fedora-packaged versions of MySQL Community Server, but in order to install and use the Oracle-provided Yum repository, DNF needs to be configured to prevent conflicts with Fedora's packages.

The `/etc/dnf/dnf.conf` file needs to be edited to include the following entry within the `[main]` section:

```text
exclude=mysql8.4*
```

## Installing Development Dependencies

In order to build Python, Ruby and Node.js from source via pyenv, rbenv and nvm, a number of dependencies must be installed.

### Fedora 40

```bash
sudo dnf groupinstall "Development Tools"
sudo dnf install zlib-devel bzip2 bzip2-devel readline-devel sqlite sqlite-devel openssl-devel xz xz-devel libffi-devel findutils tk-devel libyaml-devel gcc-g++ cmake scdoc
```

### Fedora 41+

```bash
sudo dnf group install development-tools
sudo dnf install zlib-devel bzip2 bzip2-devel readline-devel sqlite sqlite-devel openssl-devel xz xz-devel libffi-devel findutils tk-devel libyaml-devel gcc-g++ cmake scdoc
```

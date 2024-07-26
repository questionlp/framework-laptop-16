# Prepping Fedora for Python, Ruby and Node.js Web Development

## Installing First Set of Packages

These packages include some basic utilities and fonts that I use regularly.

```bash
sudo dnf install htop screen vim neofetch mozilla-fira-mono-fonts mozilla-fira-sans-fonts fira-code-fonts jetbrains-mono-fonts-all cascadia-fonts-all cascadia-code-fonts cascadia-code-pl-fonts ibm-plex-fonts-all git gh zsh avahi-tools
```

## KDE Spin of Fedora: Removing MariaDB

Since I prefer to install MySQL Community Server from the Oracle MySQL Yum repository, I first need to remove MariaDB, which is installed as a dependency for several KDE applications. Removing MariaDB will break Akonadi, Kontact and KOrganizer. That said, I don't plan on using those applications.

```bash
sudo dnf remove mariadb
```

## Installing Development Dependencies

In order to build Python, Ruby and Node.js from source via pyenv, rbenv and nvm, a number of dependencies must be installed.

```bash
sudo dnf groupinstall "Development Tools"
sudo dnf install zlib-devel bzip2 bzip2-devel readline-devel sqlite sqlite-devel openssl-devel xz xz-devel libffi-devel findutils tk-devel libyaml-devel gcc-g++
```

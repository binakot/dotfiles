# MX Linux (based on Debian stable)

## Info

```
MX-19
OS: Debian 10 buster
Shell: bash
DE: XFCE
WM: Xfwm4
```

MX Linux is a cooperative venture between the antiX and former MEPIS communities,using the best tools and talents from each distro. 
It is a midweight OS designed to combine an elegant and efficient desktop with simple configuration, high stability, solid performance and medium-sized footprint.

[Homepage](https://mxlinux.org)

[Wiki](https://en.wikipedia.org/wiki/MX_Linux)

---

## Setup

### Update system

```bash
sudo apt-get update && \
sudo apt-get -y dist-upgrade && \
sudo apt-get -y autoremove
```

---

### Install tools

```bash
sudo apt-get -y install \
     build-essential \
     curl wget
```

#### Git

TODO

---

### Install software (deb-packages)

* [Google Chomre](https://www.google.com/intl/ru/chrome/)

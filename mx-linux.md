# MX Linux (based on Debian stable)

## Info

```
MX-19
OS: Debian 10 buster
Shell: zsh
DE: XFCE
WM: Xfwm4
```

MX Linux is a cooperative venture between the antiX and former MEPIS communities,using the best tools and talents from each distro. 
It is a midweight OS designed to combine an elegant and efficient desktop with simple configuration, high stability, solid performance and medium-sized footprint.

[Homepage](https://mxlinux.org)

[Wiki](https://en.wikipedia.org/wiki/MX_Linux)

---

## Setup

---

### Update system

```bash
$ sudo apt-get update && \
       apt-get -y dist-upgrade && \
       apt-get -y autoremove
```

---

### Install tools

```bash
$ sudo apt-get -y install \
    build-essential \
    curl wget
```

#### Git

Install:

```bash
$ sudo apt-get -y install git
```

Setup SSH keys:

```bash
$ ssh-keygen -t rsa -b 4096
```

https://help.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account

https://confluence.atlassian.com/bitbucketserver/ssh-access-keys-for-system-use-776639781.html

Configure git:

```bash
$ git config --global -e
$ git config --global user.name “binakot”
$ git config --global user.email “binakot@gmail.com”
$ git config --global core.editor "vim"
```

#### Zsh + Oh My Zsh

Install:

```bash
$ sudo apt-get install zsh
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Theme:

```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git $ZSH_CUSTOM/themes/powerlevel10k
```

Set `ZSH_THEME=powerlevel10k/powerlevel10k` in your `~/.zshrc`.

#### Tmux

Install:

```bash
$ sudo apt-get install tmux
```

#### (Neo)Vim

Install:

```bash
$ sudo apt-get install neovim
```

---

### Install software (deb-packages)

* [Google Chrome](https://www.google.com/intl/ru/chrome/)

* [Telegram](https://desktop.telegram.org/)

```bash
$ wget -O- https://telegram.org/dl/desktop/linux | sudo tar xJ -C /opt/
$ sudo ln -s /opt/Telegram/Telegram /usr/local/bin/telegram
```

---

### Misc

```bash
$ sudo apt-get -y install fonts-firacode
```

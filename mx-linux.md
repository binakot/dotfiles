# MX Linux (based on Debian stable)

## Info

```
MMMMMMMMMMMMMMMMMMMMMMMMMMMMMMNMMMMMMMMM   muratov@muratov-linux-pc
MMMMMMMMMMNs..yMMMMMMMMMMMMMm: +NMMMMMMM   ------------------------
MMMMMMMMMN+    :mMMMMMMMMMNo` -dMMMMMMMM   OS: MX x86_64
MMMMMMMMMMMs.   `oNMMMMMMh- `sNMMMMMMMMM   Kernel: 4.x-amd64
MMMMMMMMMMMMN/    -hMMMN+  :dMMMMMMMMMMM   Shell: zsh
MMMMMMMMMMMMMMh-    +ms. .sMMMMMMMMMMMMM   DE: Xfce
MMMMMMMMMMMMMMMN+`   `  +NMMMMMMMMMMMMMM   WM: Xfwm4
MMMMMMMMMMMMMMNMMd:    .dMMMMMMMMMMMMMMM   WM Theme: Matcha-dark-azul
MMMMMMMMMMMMm/-hMd-     `sNMMMMMMMMMMMMM   Theme: Matcha-dark-azul [GTK2], Adwaita [GTK3]
MMMMMMMMMMNo`   -` :h/    -dMMMMMMMMMMMM   Icons: Papirus-Dark [GTK2], Adwaita [GTK3]
MMMMMMMMMd:       /NMMh-   `+NMMMMMMMMMM   
MMMMMMMNo`         :mMMN+`   `-hMMMMMMMM   
MMMMMMh.            `oNMMd:    `/mMMMMMM   
MMMMm/                -hMd-      `sNMMMM   
MMNs`                   -          :dMMM   
Mm:                                 `oMM   
MMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMM
```

MX Linux is a cooperative venture between the antiX and former MEPIS communities,using the best tools and talents from each distro. 
It is a midweight OS designed to combine an elegant and efficient desktop with simple configuration, high stability, solid performance and medium-sized footprint.

[MX Linux - Homepage](https://mxlinux.org)

[MX Linux - Wikipedia](https://en.wikipedia.org/wiki/MX_Linux)

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
    curl wget xclip \
    fonts-firacode fonts-powerline \
    lintian
```

#### Git

Install:

```bash
$ sudo apt-get -y install git
```

Setup SSH keys:

```bash
$ ssh-keygen -t rsa -b 4096 -C "binakot@github.com" # fileName: binakot.github.id_rsa
$ ssh-keygen -t rsa -b 4096 -C "binakot@bitbucket.org" # fileName: binakot.bitbucket.id_rsa
$ ssh-add -k binakot.github.id_rsa binakot.bitbucket.id_rsa
$ ssh-add -l
```

Configure git:

```bash
$ git config --global -e
```

```text
# https://git-scm.com/docs/git-config

[user]
    name = Ivan Muratov
    email = binakot@gmail.com

[github]
    user = binakot

[core]
    editor = nvim
    excludesfile = ~/.gitignore_global
    fileMode = false
    quotepath = false

[credential]
    helper = cache

[fetch]
    prune = true
    pruneTags = true

[push]
    default = simple
    followTags = true

[rebase]
    autoStash = true

[merge]
    log = true

[color]
    ui = true

[grep]
    lineNumber = true

[alias]
    config = config --list
    aliases = config --get-regexp alias
    tree = log --graph --decorate --pretty=oneline --abbrev-commit --all
```

Initial `.gitignore_global` you can take from [here](https://gist.github.com/octocat/9257657)

#### Zsh

Install:

```bash
$ sudo apt-get install zsh
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Theme:

```bash
git clone https://github.com/romkatv/powerlevel10k.git $ZSH_CUSTOM/themes/powerlevel10k
```

Set `ZSH_THEME="powerlevel10k/powerlevel10k"` in your `~/.zshrc`.

Configure fonts for terminal with patched 
[Meslo Nerd Font](https://github.com/romkatv/powerlevel10k#recommended-meslo-nerd-font-patched-for-powerlevel10k). 
Copy all `ttf` files to `/usr/share/fonts/truetype/meslonerd`.

Plugins:

* [Built-in](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins): `git tmux vi-mode docker`

* [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions): 

```bash
$ git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
# plugins=(zsh-autosuggestions)
```

* [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting):

```bash
$ git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
# plugins=(zsh-syntax-highlighting) # Note that zsh-syntax-highlighting must be the last plugin sourced.
```

The complete list of plugins:

```
plugins=(git tmux vi-mode docker zsh-autosuggestions zsh-syntax-highlighting)
```

#### Tmux

Install [Tmux](https://github.com/tmux/tmux):

```bash
$ sudo apt-get install tmux
```

Install [Tmux Plugin Manager](https://github.com/tmux-plugins/tpm):

```bash
$ git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

Configure the file `~/.tmux.conf`:

```bash
# Remap prefix from 'Ctrl-b' to 'Ctrl-a'
unbind C-b
set -g prefix C-a
bind C-a send-prefix
bind-key C-a last-window

# Enable mouse support
set -g mouse on

# Enable vi mode
set -g status-keys vi
setw -g mode-keys vi
bind-key -T copy-mode-vi v send-keys -X begin-selection
bind-key -T copy-mode-vi y send-keys -X copy-selection
bind-key -T copy-mode-vi r send-keys -X rectangle-toggle
bind -T copy-mode-vi y send-keys -X copy-pipe-and-cancel 'xclip -in -selection clipboard'

# Start windows numbering at 1
set -g base-index 1
setw -g pane-base-index 1

# Config windows behaviour
set -g renumber-windows on
setw -g automatic-rename on

# Activities
set -g monitor-activity on
set -g visual-activity off

# Split panes using | and -
bind | split-window -h
bind - split-window -v
unbind '"'
unbind %

# Switch panes using Alt-arrow without prefix
bind -n M-Left select-pane -L
bind -n M-Right select-pane -R
bind -n M-Up select-pane -U
bind -n M-Down select-pane -D

# List of plugins
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'

set -g @plugin 'jimeh/tmux-themepack'
set -g @themepack 'powerline/default/cyan'

set -g @plugin 'tmux-plugins/tmux-resurrect'
set -g @plugin 'tmux-plugins/tmux-continuum'

# Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf)
run -b '~/.tmux/plugins/tpm/tpm'
```

Reload config `$ tmux source ~/.tmux.conf`
and install the plugins in tmux with `Ctrl+A -> I`.
Later for plugins updates use `Ctrl+A -> U`.

And start to use:

```bash
$ tmux new -s dev # create session
$ tmux ls # list of sessions
$ tmux attach -t dev # attach to session
$ tmux detach # detach from session 
```

#### (Neo)Vim

Install:

```bash
$ sudo apt-get install neovim
```

Configure: 

**TODO**

---

### Install software

* [SDKMAN!](https://dzone.com/articles/sdkman-managing-sdks-were-never-so-smart) for Java, Kotlin, etc:

```bash
$ curl -s "https://get.sdkman.io" | bash
$ sdk list java             # list of available builds
$ sdk install java          # install the latest LTS build
$ sdk current java          # show the Java version for the current terminal
$ sdk use java $VERSION     # switch the Java version for the current terminal
$ sdk default java $VERSION # set a specific version as default for all terminals
```

* [Docker](https://docs.docker.com/install/linux/docker-ce/debian/)

* [JetBrains Toolbox](https://www.jetbrains.com/toolbox-app/) for Intellij IDEA and so on

* [Google Chrome](https://www.google.com/intl/ru/chrome/)

* [Slack](https://slack.com/intl/en-ru/downloads/instructions/ubuntu)

* [Telegram](https://desktop.telegram.org/):

```bash
$ wget -O- https://telegram.org/dl/desktop/linux | sudo tar xJ -C /opt/
$ sudo ln -s /opt/Telegram/Telegram /usr/local/bin/telegram
```

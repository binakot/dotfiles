# MX Linux (based on Debian stable)

## Info

```
MMMMMMMMMMMMMMMMMMMMMMMMMMMMMMNMMMMMMMMM   muratov@muratov-linux-pc 
MMMMMMMMMMNs..yMMMMMMMMMMMMMm: +NMMMMMMM   ------------------------ 
MMMMMMMMMN+    :mMMMMMMMMMNo` -dMMMMMMMM   OS: MX x86_64 
MMMMMMMMMMMs.   `oNMMMMMMh- `sNMMMMMMMMM   Kernel: 4.19.0-6-amd64 
MMMMMMMMMMMMN/    -hMMMN+  :dMMMMMMMMMMM   Shell: zsh 5.7.1 
MMMMMMMMMMMMMMh-    +ms. .sMMMMMMMMMMMMM   DE: Xfce 
MMMMMMMMMMMMMMMN+`   `  +NMMMMMMMMMMMMMM   WM: Xfwm4 
MMMMMMMMMMMMMMNMMd:    .dMMMMMMMMMMMMMMM   WM Theme: Arc-Dark 
MMMMMMMMMMMMm/-hMd-     `sNMMMMMMMMMMMMM   Theme: Greybird-mx [GTK2], Adwaita [GTK3] 
MMMMMMMMMMNo`   -` :h/    -dMMMMMMMMMMMM   Icons: Papirus [GTK2], Adwaita [GTK3]  
MMMMMMMMMd:       /NMMh-   `+NMMMMMMMMMM   Terminal: xfce4-terminal  
MMMMMMMNo`         :mMMN+`   `-hMMMMMMMM   Terminal Font: MesloLGS NF 11  
MMMMMMh.            `oNMMd:    `/mMMMMMM   
MMMMm/                -hMd-      `sNMMMM   
MMNs`                   -          :dMMM   
Mm:                                 `oMM    
MMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMM    
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

$ sudo apt-get -y install \
    fonts-firacode fonts-powerline \
    xclip lintian
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
$ git config --global user.name binakot
$ git config --global user.email binakot@gmail.com
$ git config --global core.editor nvim
```

#### Zsh + Oh My Zsh

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

Configure fonts for terminal with patched `Meslo Nerd Font` (copy `ttf` files to `/usr/share/fonts/truetype/meslonerd`): 
https://github.com/romkatv/powerlevel10k#recommended-meslo-nerd-font-patched-for-powerlevel10k

Plugins:

* [Built-in](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins)

```text
git tmux vi-mode docker
```

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
and install the plugins in tmux press `Ctrl+A -> I`.
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

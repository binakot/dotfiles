# Binakot's PC

```bash
                             binakot@binakot-pc
                             OS: Pop 21.10 impish
                             Kernel: x86_64 Linux 5.15.8-76051508-generic
         #####               Uptime: 6m
        #######              Packages: Unknown
        ##O#O##              Shell: zsh 5.8
        #######              Resolution: 3440x1440
      ###########            DE: GNOME 40.0
     #############           WM: Mutter
    ###############          WM Theme: Pop
    ################         GTK Theme: Pop-dark [GTK2/3]
   #################         Icon Theme: Pop
 #####################       Font: Fira Sans Semi-Light 10
 #####################       Disk: 12G / 454G (3%)
   #################         CPU: Intel Core i7-5820K @ 12x 4GHz [46.0°C]
                             GPU: NVIDIA GeForce GTX 1060 6GB
                             RAM: 2766MiB / 48098MiB

            /////////////                binakot@binakot-pc
         /////////////////////            ------------------
      ///////*767////////////////         OS: Pop!_OS 21.10 x86_64
    //////7676767676*//////////////       Kernel: 5.15.8-76051508-generic
   /////76767//7676767//////////////      Uptime: 7 mins
  /////767676///*76767///////////////     Packages: 1929 (dpkg)
 ///////767676///76767.///7676*///////    Shell: zsh 5.8
/////////767676//76767///767676////////   Resolution: 3440x1440
//////////76767676767////76767/////////   DE: GNOME 40.5
///////////76767676//////7676//////////   WM: Mutter
////////////,7676,///////767///////////   WM Theme: Pop
/////////////*7676///////76////////////   Theme: Pop-dark [GTK2/3]
///////////////7676////////////////////   Icons: Pop [GTK2/3]
 ///////////////7676///767////////////    Terminal: gnome-terminal
  ///////////////////////////////////     CPU: Intel i7-5820K (12) @ 4.000GHz
   //////.7676767676767676767,//////      GPU: NVIDIA GeForce GTX 1060 6GB
    /////767676767676767676767/////       Memory: 2227MiB / 48098MiB
      ///////////////////////////
         /////////////////////
             /////////////
```

## Install OS

[Pop!_OS](https://pop.system76.com/) 21.10
 
## Initial configuration

* First update

```bash
$ sudo apt-get -y update
$ sudo apt-get -y dist-upgrade
$ sudo apt -y autoremove
```

* Install utils

```bash
$ sudo apt-get install -y \
    build-essential apt-transport-https ca-certificates gnupg lsb-release \
    curl wget \
    htop iotop nmon \
    screenfetch neofetch \
    mc

$ sudo apt-get install lm-sensors &&\
    sudo sensors-detect &&\
    sensors
```

---

## SSH

Generate SSH key:

```bash
$ ssh-keygen -t ed25519

$ eval `ssh-agent -s`
$ ssh-add ~/.ssh/id_ed25519
$ ssh-add -l
```

Create and configure `~/.ssh/config`:

```text
Host github.com
    HostName github.com
    User binakot
    IdentityFile ~/.ssh/id_ed25519

Host gitlab.com
    HostName gitlab.com
    User binakot
    IdentityFile ~/.ssh/id_ed25519

Host bitbucket.org
    HostName bitbucket.org
    User binakot
    IdentityFile ~/.ssh/id_ed25519
```

---

## Git

Install `git`:

```bash
$ sudo apt-get install -y git
```

Configure `git`:

```bash
$ git config --global -e
```

```text
[user]
    name = Ivan Muratov
    email = binakot@gmail.com
```

---

## ZSH

Install `ZSH` as default shell:

```bash
$ sudo apt-get install -y zsh
$ zsh --version
$ chsh -s $(which zsh)
$ gnome-session-quit
```

After re-login check that `ZSH` is default shell:

```bash
$ echo $SHELL
$ $SHELL --version
```

Now install `oh-my-zsh` framework:

```bash
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Plugins:

```bash
$ git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:-${ZSH:-~/.oh-my-zsh}/custom}/plugins/zsh-completions

$ git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

$ git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

$ git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
$ ~/.fzf/install
```

```bash
$ nano ~/.zshrc
plugins=(
  git
  zsh-completions
  zsh-autosuggestions
  zsh-syntax-highlighting  
  fzf
)
```

Theme:

Install patched fonts: https://github.com/romkatv/powerlevel10k#manual-font-installation.

Then install `powerlevel10k` and configure it:

```bash
$ git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k

$ nano ~/.zshrc
ZSH_THEME="powerlevel10k/powerlevel10k"

$ p10k configure
yyyy3121111121n1y
```

---

## Software

* [Docker](https://www.docker.com/)

```bash
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

$ echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  
$ sudo apt-get update
$ sudo apt-get install -y docker-ce docker-ce-cli containerd.io
$ sudo docker run --rm hello-world
```

```bash
$ sudo usermod -aG docker ${USER}
$ su - ${USER}
$ docker --version
```

```bash
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
$ sudo chmod +x /usr/local/bin/docker-compose
$ docker-compose --version
```

* [SDKMAN!](https://sdkman.io/)

```bash
$ curl -s "https://get.sdkman.io" | bash

$ sdk list java             # list of available builds
$ sdk install java          # install the latest LTS build
$ sdk current java          # show the Java version for the current terminal

$ sdk use java $VERSION     # switch the Java version for the current terminal
$ sdk default java $VERSION # set a specific version as default for all terminals
```

* [NVM](https://github.com/nvm-sh/nvm)

```bash
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash

$ nvm ls-remote | grep "Latest LTS" # list available versions of Node
$ nvm install --lts                 # install the latest LTS release of Node
$ nvm current                       # show the Node version for the current terminal
```

* [Python](https://www.python.org/)

```bash
$ sudo apt-get install -u \
    python3-dev python3-pip python3-venv
```

* [Google Chrome](https://www.google.com/intl/ru/chrome/)

* [JetBrains Toolbox](https://www.jetbrains.com/toolbox-app/)

* [Telegram](https://desktop.telegram.org/)

---

## Misc

* Configure `~/.gitignore_global`:

```text
# Compiled source #
###################
*.com
*.class
*.dll
*.exe
*.o
*.so

# Packages #
############
*.7z
*.dmg
*.gz
*.iso
*.jar
*.rar
*.tar
*.zip

# Logs and databases #
######################
*.log
*.sql
*.sqlite

# OS generated files #
######################
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db
```

* Configure `~/.editorconfig`:

```text
root = true

[*]
charset = utf-8
end_of_line = lf
indent_style = space
indent_size = 2
continuation_indent_size = 2
insert_final_newline = true
trim_trailing_whitespace = true

[*.md]
trim_trailing_whitespace = false

[*.java]
indent_size = 4
continuation_indent_size = 4
```

---

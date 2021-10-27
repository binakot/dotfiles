# Binakot PC

```bash
binakot@binakot-pc:~$ screenfetch
                             binakot@binakot-pc
                             OS: Pop 20.04 focal
                             Kernel: x86_64 Linux 5.13.0-7614-generic
         #####               Uptime: 14m
        #######              Packages: Unknown
        ##O#O##              Shell: bash 5.0.17
        #######              Resolution: 3440x1440
      ###########            DE: GNOME 3.38.3
     #############           WM: Mutter
    ###############          WM Theme: Pop
    ################         GTK Theme: Pop-dark [GTK2/3]
   #################         Icon Theme: Pop
 #####################       Font: Fira Sans Semi-Light 10
 #####################       Disk: 8,1G / 130G (7%)
   #################         CPU: Intel Core i7-5820K @ 12x 4,3GHz [47.0°C]
                             GPU: NVIDIA GeForce GTX 1060 6GB
                             RAM: 2713MiB / 48100MiB

binakot@binakot-pc:~$ neofetch
             /////////////                binakot@binakot-pc 
         /////////////////////            ------------------ 
      ///////*767////////////////         OS: Pop!_OS 20.04 LTS x86_64 
    //////7676767676*//////////////       Kernel: 5.13.0-7614-generic 
   /////76767//7676767//////////////      Uptime: 14 mins 
  /////767676///*76767///////////////     Packages: 1808 (dpkg) 
 ///////767676///76767.///7676*///////    Shell: bash 5.0.17 
/////////767676//76767///767676////////   Resolution: 3440x1440 
//////////76767676767////76767/////////   DE: GNOME 
///////////76767676//////7676//////////   WM: Mutter 
////////////,7676,///////767///////////   WM Theme: Pop 
/////////////*7676///////76////////////   Theme: Pop-dark [GTK2/3] 
///////////////7676////////////////////   Icons: Pop [GTK2/3] 
 ///////////////7676///767////////////    Terminal: gnome-terminal 
  //////////////////////'////////////     CPU: Intel i7-5820K (12) @ 4.300GHz 
   //////.7676767676767676767,//////      GPU: NVIDIA GeForce GTX 1060 6GB 
    /////767676767676767676767/////       Memory: 2202MiB / 48100MiB 
      ///////////////////////////
         /////////////////////                                    
             /////////////                                        
```

## Install OS

[Pop!_OS](https://pop.system76.com/) 20.04 LTS
 
## Initial configuration

* First update

```bash
$ sudo apt-get update
$ sudo apt-get dist-upgrade
$ sudo apt autoremove
```

---

## Install software

* Utils

```bash
$ sudo apt-get install \
    mc \
    curl wget \
    htop iotop nmon \
    screenfetch neofetch \
    cloc
```

* [Docker](https://www.docker.com/)

```bash
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

$ echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
$ sudo docker run hello-world
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
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

$ nvm ls-remote             # list available versions of Node
$ nvm install --lts         # install the latest LTS release of Node
$ nvm current               # show the Node version for the current terminal
```

* [Google Chrome](https://www.google.com/intl/ru/chrome/)

* [JetBrains Toolbox](https://www.jetbrains.com/toolbox-app/)

* [Slack](https://slack.com/intl/en-ru/downloads/instructions/ubuntu)

* [Telegram](https://desktop.telegram.org/)

```bash
$ sudo apt-get install telegram-desktop
```

---

## SSH

Generate SSH key:

```bash
$ ssh-keygen -t ed25519
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
    User muratovii
    IdentityFile ~/.ssh/id_ed25519
```

## Git

Install `git`:

```bash
$ sudo apt-get install git
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

Configure `~/.gitignore_global`:

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

## ZSH

> TODO

## Others

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

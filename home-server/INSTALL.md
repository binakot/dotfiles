# Home Server

```bash
user@home-server:~$ screenfetch
                          ./+o+-       user@home-server
                  yyyyy- -yyyyyy+      OS: Ubuntu 20.04 focal
               ://+//////-yyyyyyo      Kernel: x86_64 Linux 5.4.0-89-generic
           .++ .:/++++++/-.+sss/`      Uptime: 2m
         .:++o:  /++++++++/:--:/-      Packages: 664
        o:+o+:++.`..```.-/oo+++++/     Shell: bash 5.0.17
       .:+o:+o/.          `+sssoo+/    Disk: 7.1G / 208G (4%)
  .++/+:+oo+o:`             /sssooo.   CPU: Intel Xeon E5-2620 v2 @ 12x 2.6GHz [32.0°C]
 /+++//+:`oo+o               /::--:.   GPU: GeForce GT 710
 \+/+o+++`o++o               ++////.   RAM: 554MiB / 7878MiB
  .++.o+++oo+:`             /dddhhh.
       .+.o+oo:.          `oddhhhh+
        \+.++o+o``-````.:ohdhhhhh+
         `:o+++ `ohhhhhhhhyo++os:
           .o:`.syhhhhhhh/.oo++o`
               /osyyyyyyo++ooo+++/
                   ````` +oo+++o\:
                          `oo++.

user@home-server:~$ neofetch
            .-/+oossssoo+/-.               user@home-server
        `:+ssssssssssssssssss+:`           ----------------
      -+ssssssssssssssssssyyssss+-         OS: Ubuntu 20.04.3 LTS x86_64
    .ossssssssssssssssssdMMMNysssso.       Kernel: 5.4.0-89-generic
   /ssssssssssshdmmNNmmyNMMMMhssssss/      Uptime: 2 mins
  +ssssssssshmydMMMMMMMNddddyssssssss+     Packages: 661 (dpkg), 4 (snap)
 /sssssssshNMMMyhhyyyyhmNMMMNhssssssss/    Shell: bash 5.0.17
.ssssssssdMMMNhsssssssssshNMMMdssssssss.   Resolution: 1600x900
+sssshhhyNMMNyssssssssssssyNMMMysssssss+   Terminal: /dev/pts/0
ossyNMMMNyMMhsssssssssssssshmmmhssssssso   CPU: Intel Xeon E5-2620 v2 (12) @ 2.600GHz
ossyNMMMNyMMhsssssssssssssshmmmhssssssso   GPU: NVIDIA GeForce GT 710
+sssshhhyNMMNyssssssssssssyNMMMysssssss+   Memory: 296MiB / 7878MiB
.ssssssssdMMMNhsssssssssshNMMMdssssssss.
 /sssssssshNMMMyhhyyyyhdNMMMNhssssssss/
  +sssssssssdmydMMMMMMMMddddyssssssss+
   /ssssssssssshdmNNNNmyNMMMMhssssss/
    .ossssssssssssssssssdMMMNysssso.
      -+sssssssssssssssssyyyssss+-
        `:+ssssssssssssssssss+:`
            .-/+oossssoo+/-.
```

## Install OS

[Ubuntu Server](https://ubuntu.com/download/server) 20.04 LTS

## Initial configuration

* First update

```bash
$ sudo apt-get update
$ sudo apt-get dist-upgrade
$ sudo apt autoremove
```

* Install `openssh-server` and enable it

```bash
$ sudo apt-get install openssh-server
```

* Enable `ufw` and allow `ssh`

```bash
$ sudo ufw enable
$ sudo ufw allow ssh
$ sudo ufw status verbose
```

---

## Install software

* Utils

```bash
$ sudo apt-get install \
    screen mc \
    curl wget \
    htop iotop nmon \
    lm-sensors \
    screenfetch neofetch
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

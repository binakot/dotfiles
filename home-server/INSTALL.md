# Home Server

## Install OS

Ubuntu Server 20.04 LTS
 
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

* Reboot

```bash
$ sudo reboot
```

---

## Install software

* Utils

```bash
$ sudo apt-get install \
    screen \
    mc \
    htop iotop nmon
```

* Docker

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

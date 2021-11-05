# Smart Home

Based on [Home Assistant](https://www.home-assistant.io/) in `Docker` environment.

## Getting Started

Pull required docker images and run apps stack via docker-compose:

```bash
$ docker-compose pull
$ docker-compose up -d
```

Open port for access to home assistant web-ui and Apple HomeKit integration:

```bash
# Hassio web-ui
$ sudo ufw allow 8123
# Hassio HomeKit integration 
$ sudo ufw allow 5353
$ sudo ufw allow 21064
```

Open `Home Assistant` at [http://home-server.lan:8123](http://home-server.lan:8123).

Also there is available [Portainer](https://github.com/portainer/portainer) 
at [http://home-server.lan:9000](http://home-server.lan:9000).

## Install Home Assistant Community Store

[HACS](https://hacs.xyz/) gives you a powerful UI to handle downloads of all your custom needs.

```bash
$ docker exec -it homeassistant /bin/bash
$ wget -O - https://get.hacs.xyz | bash -
$ exit
```

Now reboot `Home Assistant` from web-ui.
And finally add new integration in configuration menu named `HACS`.
Initial startup may take more then one hour because it uses GitHub API with rate limit.

## Install HA integrations

* [Xiaomi Gateway 3 for Home Assistant](https://github.com/AlexxIT/XiaomiGateway3) by @AlexxIT.

* [Aqara Gateway for Home Assistant](https://github.com/niceboygithub/AqaraGateway) by @niceboygithub.

* [Yandex Smart Home for Home Assistant](https://github.com/dmitry-k/yandex_smart_home) by @dmitry-k.

* [Yandex Station for Home Assistant](https://github.com/AlexxIT/YandexStation) by @AlexxIT.

* [Yandex Dialogs for Home Assistant](https://github.com/AlexxIT/YandexDialogs) by @AlexxIT.

* [HomeKit integration](https://www.home-assistant.io/integrations/homekit/)

---

## Misc

To restart the stack use:

```bash
$ docker-compose restart
```

To update the stack use:

```bash
$ docker-compose pull
$ docker-compose up -d
```

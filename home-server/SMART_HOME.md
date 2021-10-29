# SMART HOME

Based on [Home Assistant](https://www.home-assistant.io/).

```bash
# Open port for access to home assistant web-ui
$ sudo ufw allow 8123

# Pull docker images and run apps stack
$ docker-compose pull
$ docker-compose up -d
```

Open `Home Assistant` at [http://home-server.lan:8123](http://home-server.lan:8123).

Also there is available [Portainer](https://github.com/portainer/portainer) 
at [http://home-server.lan:9000](http://home-server.lan:9000).

---

To restart the stack use:

```bash
$ docker-compose restart
```

---

To update the stack use:

```bash
$ docker-compose pull
$ docker-compose up -d
```

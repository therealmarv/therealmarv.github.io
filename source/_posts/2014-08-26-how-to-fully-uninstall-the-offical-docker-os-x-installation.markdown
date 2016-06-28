---
layout: post
title: "How to fully uninstall the official Docker OS X installation"
date: 2014-08-26 00:41:46 +0200
comments: true
categories: [docker, uninstall]
---

No offense against [Docker](https://www.docker.com). I like the concept and the software!

*- This guide works for docker toolbox and old boot2docker, some boot2docker steps uninstall steps are not needed but it will not hurt for docker toolbox uninstallation -*

~~But I absolutely do **not** like the official Docker OS X installer~~ ([install manual](https://docs.docker.com/installation/mac/)). Things are improved with docker toolbox but uninstalling is still not trivial.

The reason for this are:

* ~~It installs Virtualbox and has downgraded my existing Virtualbox Host software~~ (seems this is fixed for Docker toolbox now).
* It installs an app and several binaries.
* Everything packaged up into a `.pkg` and no uninstall app or instructions anywhere!
<!-- more -->
In summary it tries to do too much. Many developers use tools like Vagrant & Homebrew. Why not go that way?

### Uninstall steps for boot2docker / Docker

Be sure you've only used the **official** installer. This uninstall guide is not the right one if you have installed Docker with e.g. Homebrew or other methods.

If you also want to delete all your docker machines first run:
```bash
docker-machine rm -f $(docker-machine ls -q);
```

Stop boot2docker and delete the VBox image:
```bash
boot2docker stop
boot2docker delete
```

Remove boot2docker & docker app:
```bash
sudo rm -rf /Applications/boot2docker
sudo rm -rf /Applications/Docker
```

Remove all Docker and boot2docker command line tools:
```bash
sudo rm -f /usr/local/bin/docker
sudo rm -f /usr/local/bin/boot2docker
sudo rm -f /usr/local/bin/docker-machine
sudo rm -r /usr/local/bin/docker-machine-driver*
sudo rm -f /usr/local/bin/docker-compose
```

Remove docker packages:
```bash
sudo pkgutil --forget io.docker.pkg.docker
sudo pkgutil --forget io.docker.pkg.dockercompose
sudo pkgutil --forget io.docker.pkg.dockermachine
sudo pkgutil --forget io.boot2dockeriso.pkg.boot2dockeriso
```

Remove boot2docker VBox image:
```bash
sudo rm -rf /usr/local/share/boot2docker

rm -rf ~/.boot2docker
```

Remove boot2docker ssh keys:
```bash
rm ~/.ssh/id_boot2docker*
```

Remove additional boot2docker files in `/private` folder:
```bash
sudo rm -f /private/var/db/receipts/io.boot2docker.*
sudo rm -f /private/var/db/receipts/io.boot2dockeriso.*
```

Remove docker toolbox config folder:
```bash
rm -rf ~/.docker
```

Remove the environmental variable `DOCKER_HOST` in case you have fixed it somewhere like e.g. in `.bash_profile`.

### (Optional) Uninstall steps for Virtualbox

You can also delete Virtualbox of course. But if you are a developer you probably need it anyway. In case your VBox got also downgraded: Reinstall Virtualbox.

If you really want to uninstall Virtualbox:

* Download the latest official [Virtualbox installer](https://www.virtualbox.org/wiki/Downloads).
* Open the DMG file and [execute the uninstaller](https://www.virtualbox.org/manual/ch02.html#idp50285088). Simple!

---

#### Update 2016-06-28:

* Also remove docker toolbox `~/.docker` directory.

#### Update 2016-06-03:

* Added newest uninstall procedures for [docker toolbox](https://github.com/docker/toolbox/blob/master/osx/uninstall.sh).

#### Update 2014-10-06:

* Added [uninstall instructions for home directory](https://github.com/boot2docker/osx-installer/issues/46#issuecomment-56329250).

#### Update 2014-11-13:

* Added ssh keys uninstall. Thx to [@mtscout6](https://twitter.com/mtscout6).

#### Update 2014-11-21:

* boot2docker has now an [official uninstall script](https://github.com/boot2docker/osx-installer/blob/master/uninstall.sh) but I do not know how you are supposed to run it except downloading it directly from github and execute it :( Today I've checked and it is not inside the [official pkg installer](https://github.com/boot2docker/osx-installer/releases). It also [does not remove the boot2docker app](https://github.com/boot2docker/osx-installer/issues/88) from your programs.
* Added uninstall routines for `/private` folder.
---
layout: post
title: "How to fully uninstall the offical Docker OS X installation"
date: 2014-08-26 00:41:46 +0200
comments: true
categories: [docker, uninstall]
---

No offense against [Docker](https://www.docker.com). I like the concept and the software!

*- This guide is based on V1.1.2 of the installer -*

But I absolutely do **not** like the official Docker OS X installer ([install manual](https://docs.docker.com/installation/mac/)) which you can download [here](https://github.com/boot2docker/osx-installer/releases).

The reason for this are:

* It installs Virtualbox and has downgraded my existing Virtualbox Host software!
* It installs an app and several binaries.
* Everything packaged up into a `.pkg` and no uninstall app or instructions anywhere!
<!-- more -->
In summary it tries to do too much. Many developers use tools like Vagrant & Homebrew. Why not go that way?

### Uninstall steps for boot2docker / Docker

Be sure you've only used the **official** installer. This uninstall guide is not the right one if you have installed Docker with e.g. Homebrew or other methods. 

First stop boot2docker and delete the VBox image:
```
boot2docker stop
boot2docker delete
```

Remove the environmental variable `DOCKER_HOST` in case you have fixed it somewhere like e.g. in `.bash_profile`

Drag and Drop the boot2docker app logo from applications into the trash can of OS X.

Remove Docker and boot2docker command line tools:
```
sudo rm /usr/local/bin/docker
sudo rm /usr/local/bin/boot2docker
```

Remove boot2docker VBox image:
```
sudo rm /usr/local/share/boot2docker/boot2docker.iso
sudo rmdir /usr/local/share/boot2docker
```

### (Optional) Uninstall steps for Virtualbox

You can also delete Virtualbox of course. But if you are a developer you probably need it anyway. In case your VBox got also downgraded: Reinstall Virtualbox.

If you really want to uninstall Virtualbox:

* Download the latest official [Virtualbox installer](https://www.virtualbox.org/wiki/Downloads).
* Open the DMG file and [execute the uninstaller](https://www.virtualbox.org/manual/ch02.html#idp50285088). Simple!

### But I like Docker, how to install it without pain on OS X and how to upgrade?

I will explain this in a later post! :)



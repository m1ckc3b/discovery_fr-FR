# Linux

Voici les commandes d'installation de quelques distributions Linux.

## Ubuntu 20.04 ou plus récent / Debian 10 ou plus récent

> **REMARQUE** `gdb-multiarch` est la commande GDB que vous utiliserez pour déboguer vos programmes ARM
> Cortex-M
``` console
$ sudo apt-get install \
gdb-multiarch \
minicom
```

## Fedora 32 ou plus récent
> **REMARQUE** `gdb` est la commande GDB que vous utiliserez pour déboguer vos programmes ARM
> Cortex-M
``` console
$ sudo dnf install \
gdb \
minicom
```

## Arch Linux

> **REMARQUE** `arm-none-eabi-gdb` est la commande GDB que vous utiliserez pour déboguer vos programmes ARM
> Cortex-M
``` console
$ sudo pacman -S \
arm-none-eabi-gdb \
minicom
```

## Autres distributions

> **REMARQUE** `arm-none-eabi-gdb` est la commande GDB que vous utiliserez pour déboguer vos programmes ARM
> Cortex-M

Pour les distributions qui n'ont pas de paquets pour la [chaîne d'outils
pré-intégrée d'ARM](https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads),
téléchargez le fichier "Linux 64 bits" et placez son répertoire `bin` au bon endroit.
Voici une façon de procéder :

``` console
$ mkdir -p ~/local && cd ~/local
$ tar xjf /path/to/downloaded/file/gcc-arm-none-eabi-9-2020-q2-update-x86_64-linux.tar.bz2
```

Ensuite, utilisez l'éditeur de votre choix pour ajouter à votre `PATH` dans le
fichier d'initialisation du shell approprié (par exemple `~/.zshrc` ou `~/.bashrc`) :

```
PATH=$PATH:$HOME/local/gcc-arm-none-eabi-9-2020-q2-update/bin
```

## règles udev

Ces règles vous permettent d'utiliser des périphériques USB comme le micro:bit sans privilège root, c'est-à-dire `sudo`.

Créez ce fichier dans `/etc/udev/rules.d` avec le contenu ci-dessous.

``` console
$ cat /etc/udev/rules.d/69-microbit.rules
```

``` texte
# CMSIS-DAP pour microbit

ACTION!="add|change", GOTO="microbit_rules_end"

SUBSYSTEM=="usb", ATTR{idVendor}=="0d28", ATTR{idProduct}=="0204", TAG+="uaccess"

LABEL="microbit_rules_end"
```

Rechargez ensuite les règles udev avec :

``` console
$ sudo udevadm control --reload
```

Si vous aviez une carte branchée sur votre ordinateur, débranchez-la puis rebranchez-la ou exécutez la commande suivante.

``` console
$ sudo udevadm trigger
```

Maintenant, passez à la [section suivante].

[section suivante] : verify.md
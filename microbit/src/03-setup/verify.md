# Vérification de l'installation

Vérifions que tous les outils ont été correctement installés.

## Linux uniquement

### Vérification des autorisations

Connectez le micro:bit à votre ordinateur à l'aide d'un câble USB.

Le micro:bit doit maintenant apparaître comme un périphérique USB (fichier) dans `/dev/bus/usb`. Voyons comment il a été
énuméré :

``` console
$ lsusb | grep -i "NXP ARM mbed"
Bus 001 Device 065: ID 0d28:0204 NXP ARM mbed
$ # ^^^ ^^^
```

Dans mon cas, le micro:bit a été connecté au bus n°1 et a été énuméré comme le périphérique n°65. Cela signifie que le
fichier `/dev/bus/usb/001/065` *est* le micro:bit. Vérifions les autorisations du fichier :

``` console
$ ls -l /dev/bus/usb/001/065
crw-rw-r--+ 1 nobody nobody 189, 64 Sep 5 14:27 /dev/bus/usb/001/065
```

Les autorisations doivent être `crw-rw-r--+`, notez le `+` à la fin, puis consultez vos droits d'accès en exécutant la commande suivante.

``` console
$ getfacl /dev/bus/usb/001/065
getfacl : suppression du caractère '/' de début des noms de chemin absolu
# file : dev/bus/usb/001/065
# owner : nobody
# group : nobody
user::rw-
user:<VOTRE-NOM-D'UTILISATEUR> :rw-
group::rw-
mask::rw-
other::r-
```

Vous devriez voir votre nom d'utilisateur dans la liste ci-dessus avec les autorisations `rw-`, sinon... vérifiez vos [règles
udev] et essayez de les recharger avec :

[règles udev] : linux.md#udev-rules

``` console
$ sudo udevadm control --reload
$ sudo udevadm trigger
```

# Tous

## Vérification de cargo-embed
Tout d'abord, connectez le micro:bit à votre ordinateur à l'aide d'un Câble USB.

Au moins une LED orange juste à côté du port USB du micro:bit doit s'allumer.
De plus, si vous n'avez jamais flashé un autre programme sur votre micro:bit, le programme par défaut fourni avec le micro:bit doit commencer à faire clignoter les LED rouges à l'arrière, vous
pouvez les ignorer.

Voyons maintenant si probe-rs, et par extension cargo-embed, peuvent voir votre micro:bit, vous pouvez le faire en exécutant la commande suivante.

``` console
$ probe-rs list
The following debug probes were found:
[0] : BBC micro:bit CMSIS-DAP -- 0d28:0204:990636020005282030f57fa14252d446000000006e052820 (CMSIS-DAP)
```

Ou si vous souhaitez plus d'informations sur les capacités de débogage de micro:bits, vous pouvez exécuter :

``` console
$ probe-rs info
Probing target via JTAG

Error identifying target using protocol JTAG: The probe does not support the JTAG protocol.

Probing target via SWD

ARM Chip with debug port Default:
Debug Port: DPv1, DP Designer: ARM Ltd
├── 0 MemoryAP
│   └── ROM Table (Class 1), Designer: Nordic VLSI ASA
│       ├── Cortex-M4 SCS   (Generic IP component)
│       │   └── CPUID
│       │       ├── IMPLEMENTER: ARM Ltd
│       │       ├── VARIANT: 0
│       │       ├── PARTNO: Cortex-M4
│       │       └── REVISION: 1
│       ├── Cortex-M3 DWT   (Generic IP component)
│       ├── Cortex-M3 FBP   (Generic IP component)
│       ├── Cortex-M3 ITM   (Generic IP component)
│       ├── Cortex-M4 TPIU  (Coresight Component)
│       └── Cortex-M4 ETM   (Coresight Component)
└── 1 Unknown AP (Designer: Nordic VLSI ASA, Class: Undefined, Type: 0x0, Variant: 0x0, Revision: 0x0)


Debugging RISC-V targets over SWD is not supported. For these targets, JTAG is the only supported protocol. RISC-V specific information cannot be printed.
Debugging Xtensa targets over SWD is not supported. For these targets, JTAG is the only supported protocol. Xtensa specific information cannot be printed.

```

Vous devrez ensuite modifier `Embed.toml` dans le répertoire `src/03-setup` du code source du
livre. Dans la section `default.general`, vous trouverez deux
variantes de puces commentées :

```toml
[default.general]
# chip = "nrf52833_xxAA" # uncomment this line for micro:bit V2
# chip = "nrf51822_xxAA" # uncomment this line for micro:bit V1
```

Si vous travaillez avec la carte micro:bit v2, décommentez la première, pour la v1
décommentez la deuxième ligne.

Ensuite, exécutez l'une de ces commandes :

```
$ # make sure you are in src/03-setup of the books source code
$ # If you are working with micro:bit v2
$ rustup target add thumbv7em-none-eabihf
$ cargo embed --target thumbv7em-none-eabihf

$ # If you are working with micro:bit v1
$ rustup target add thumbv6m-none-eabi
$ cargo embed --target thumbv6m-none-eabi

```

Si tout fonctionne correctement, cargo-embed doit d'abord compiler le petit programme d'exemple
dans ce répertoire, puis le flasher et enfin ouvrir une belle interface utilisateur textuelle qui
imprime Hello World.

(Si ce n'est pas le cas, consultez les instructions de [dépannage général].)

[dépannage général] : ../appendix/1-general-troubleshooting/index.html

Ce résultat provient du petit programme Rust que vous venez de flasher sur votre micro:bit.
Tout fonctionne correctement et vous pouvez continuer avec les chapitres suivants !
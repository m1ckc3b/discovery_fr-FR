# Windows

## `arm-none-eabi-gdb`

ARM fournit des installateurs `.exe` pour Windows. Prenez-en un depuis [ici][gcc] et suivez les instructions.
Juste avant la fin du processus d'installation, cochez/sélectionnez l'option "Ajouter un chemin à la variable d'environnement"
. Vérifiez ensuite que les outils se trouvent dans votre `%PATH%` :

``` console
$ arm-none-eabi-gcc -v
(..)
gcc version 5.4.1 20160919 (release) (..)
```

[gcc] : https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads

## PuTTY

Téléchargez le dernier `putty.exe` depuis [ce site] et placez-le quelque part dans votre `%PATH%`.

[ce site] : http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html

Maintenant, allez à la [section suivante].

[section suivante] : verify.md
# Roulette LED

Très bien, commençons par créer l'application suivante :

<p align="center">
<video src="../assets/roulette_fast.mp4" loop autoplay>
</p>

Je vais vous donner une API de haut niveau pour implémenter cette application, mais ne vous inquiétez pas, nous ferons des choses de bas niveau plus tard. L'objectif principal de ce chapitre est de vous familiariser avec le processus de *clignotement* et de débogage.

Le code de démarrage se trouve dans le répertoire `src` du référentiel du livre. À l'intérieur de ce répertoire, il y a d'autres
répertoires nommés d'après chaque chapitre de ce livre. La plupart de ces répertoires sont des
projets Cargo de démarrage.

Maintenant, accédez au répertoire `src/05-led-roulette`. Vérifiez le fichier `src/main.rs` :

``` rust
{{#include src/main.rs}}
```

Les programmes de microcontrôleurs sont différents des programmes standards sur deux aspects : `#![no_std]` et
`#![no_main]`.

L'attribut `no_std` indique que ce programme n'utilisera pas la crate `std`, qui suppose un
système d'exploitation sous-jacent ; le programme utilisera à la place la crate `core`, un sous-ensemble de `std` qui peut s'exécuter sur des
systèmes bare metal (c'est-à-dire des systèmes sans abstractions de système d'exploitation comme les fichiers et les sockets).

L'attribut `no_main` indique que ce programme n'utilisera pas l'interface `main` standard, qui
est adaptée aux applications en ligne de commande qui reçoivent des arguments. Au lieu de l'interface `main` standard, nous
utiliserons l'attribut `entry` de la crate [`cortex-m-rt`] pour définir un point d'entrée personnalisé. Dans ce
programme, nous avons nommé le point d'entrée "main", mais tout autre nom aurait pu être utilisé. La fonction de point d'entrée
doit avoir la signature `fn() -> !` ; ce type indique que la fonction ne retourne rien
-- cela signifie que le programme ne se termine jamais.

[`cortex-m-rt`] : https://crates.io/crates/cortex-m-rt

Si vous êtes un observateur attentif, vous remarquerez également qu'il existe un répertoire `.cargo`. Ce répertoire contient un fichier de configuration Cargo (`.cargo/config`) qui modifie le
processus de liaison pour adapter la disposition de la mémoire du programme aux exigences du périphérique cible.
Ce processus de liaison modifié est une exigence de la crate `cortex-m-rt`.

De plus, il existe également un fichier `Embed.toml`

```toml
{{#include Embed.toml}}
```

Ce fichier indique à `cargo-embed` que :

* nous travaillons avec un nrf52833 ou un nrf51822, vous devrez à nouveau supprimer les commentaires de la
puce que vous utilisez, comme vous l'avez fait au chapitre 3.
* nous voulons arrêter la puce après l'avoir flashée afin que notre programme ne saute pas instantanément dans la boucle
* nous voulons désactiver RTT, qui est un protocole permettant à la puce d'envoyer du texte à un débogueur.
Vous avez en fait déjà vu RTT en action, c'était le protocole qui a envoyé "Hello World" au chapitre 3.
* nous voulons activer GDB, cela sera nécessaire pour la procédure de débogage

Très bien, commençons par construire ce programme.
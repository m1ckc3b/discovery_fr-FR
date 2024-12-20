# Terminologie de Rust Embedded
Avant de nous plonger dans la programmation du micro:bit, jetons un coup d'œil
rapide aux bibliothèques et à la terminologie qui seront importantes pour tous les
chapitres à venir.

## Couches d'abstraction
Pour tout microcontrôleur/carte entièrement pris en charge avec un microcontrôleur
vous entendrez généralement les termes suivants utilisés pour leurs
niveaux d'abstraction :

### Boîtier d'accès périphérique (PAC - Peripheral Access Crate)
Le travail du PAC est de fournir une interface directe (plutôt) sûre aux
périphériques de la puce, vous permettant de configurer
chaque dernier bit comme vous le souhaitez (bien sûr également de manière incorrecte). En général,
vous n'avez à traiter avec le PAC que si les couches
qui sont plus hautes ne répondent pas à vos besoins ou lorsque vous les développez.
Le PAC que nous allons (implicitement) utiliser est soit celui du [nRF52]
soit celui du [nRF51].

### La couche d'abstraction matérielle (HAL - Hardware Abstraction Layer)
Le rôle de la couche HAL est de s'appuyer sur
le PAC de la puce et de fournir une abstraction réellement utilisable par
quelqu'un qui ne connaît pas tout le comportement spécial de cette puce.
En général, ils abstraitent des périphériques entiers dans des structures uniques qui peuvent
par exemple être utilisées pour envoyer des données via le périphérique. Nous allons
utiliser respectivement le [nRF52-hal] ou le [nRF51-hal].

### Le Board Support Crate (historiquement appelé Board Support Package, ou BSP)
Le rôle du BSP est d'abstraire une carte entière
(comme le micro:bit) en une seule fois. Cela signifie qu'il doit fournir
des abstractions pour utiliser à la fois le microcontrôleur ainsi que les capteurs,
les LED, etc. qui peuvent être présents sur la carte. Très souvent (en particulier
avec des cartes personnalisées), vous travaillerez simplement avec un HAL pour la
puce et construirez les pilotes pour les capteurs vous-même ou
les rechercherez sur crates.io. Heureusement pour nous, le micro:bit
dispose en fait d'un [BSP], nous allons donc l'utiliser en plus de notre
HAL également.

[nrF52] : https://crates.io/crates/nrf52833-pac
[nrF51] : https://crates.io/crates/nrf51
[nrF52-hal] : https://crates.io/crates/nrf52833-hal
[nrF51-hal] : https://crates.io/crates/nrf51-hal
[BSP] : https://crates.io/crates/microbit

## Unifier les couches

Nous allons maintenant examiner un élément logiciel très central
dans le monde de Rust Embedded : [`embedded-hal`]. Comme son nom l'indique, il
fait référence au 2e niveau d'abstraction que nous avons appris à connaître : les HAL.
L'idée derrière [`embedded-hal`] est de fournir un ensemble de traits qui
décrivent le comportement qui est généralement partagé par toutes les implémentations
d'un périphérique spécifique dans tous les HAL. Par exemple, on s'attendrait toujours
à avoir des fonctions capables d'allumer ou d'éteindre une broche. Par exemple, pour allumer et éteindre une LED sur la carte.
Cela nous permet d'écrire un pilote pour, par exemple, un capteur de température, qui
peut être utilisé sur n'importe quelle puce pour laquelle une implémentation des traits [`embedded-hal`] existe,
simplement en écrivant le pilote de telle manière qu'il ne repose que sur les
traits [`embedded-hal`]. Les pilotes qui sont écrits de cette manière sont dits
indépendants de la plate-forme et heureusement pour nous, la plupart des pilotes sur crates.io
sont en fait indépendants de la plate-forme.

[`embedded-hal`] : https://crates.io/crates/embedded-hal

## Lectures complémentaires

Si vous souhaitez en savoir plus sur ces niveaux d'abstraction, Franz Skarman,
alias [TheZoq2], a tenu une conférence sur ce sujet lors d'Oxidize 2020, intitulée
[An Overview of the Embedded Rust Ecosystem].

[TheZoq2] : https://github.com/TheZoq2/
[An Overview of the Embedded Rust Ecosystem] : https://www.youtube.com/watch?v=vLYit_HHPaY
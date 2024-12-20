# Flashe-le

Le flashage est le processus de déplacement de notre programme dans la mémoire (persistante) du microcontrôleur. Une fois
flashé, le microcontrôleur exécutera le programme flashé à chaque fois qu'il sera mis sous tension.

Dans ce cas, notre programme `led-roulette` sera le *seul* programme dans la mémoire du microcontrôleur.
Je veux dire par là qu'il n'y a rien d'autre qui tourne sur le microcontrôleur : pas d'OS, pas de " daemon ",
rien. `led-roulette` a le contrôle total sur l'appareil.

Le flashage du binaire lui-même est assez simple grâce à `cargo embed`.

Avant d'exécuter cette commande, voyons ce qu'elle fait réellement. Si vous regardez le côté de votre micro:bit
avec le connecteur USB tourné vers le haut, vous remarquerez qu'il y a en fait 2 carrés noirs dessus
(sur le micro:bit v2, il y en a même un troisième, un peu plus grand, qui est un haut-parleur), l'un est notre MCU
dont nous avons déjà parlé mais à quoi sert l'autre ? L'autre puce a 3 objectifs principaux :

1. Fournir l'alimentation du connecteur USB à notre MCU
2. Fournir un pont série vers USB pour notre MCU (nous verrons cela dans un chapitre ultérieur)
3. Être un programmeur/débogueur (c'est l'objectif pertinent pour l'instant)

En gros, cette puce agit comme un pont entre notre ordinateur (auquel elle est connectée via USB) et le MCU (auquel elle est
connectée via les circuits imprimés sur la carte et communique avec en utilisant le protocole SWD). Ce pont nous permet de flasher de nouveaux binaires sur
le MCU, d'inspecter son état via un débogueur et d'autres choses.

Alors flashons-le !

```console
# Pour micro:bit v2
$ cargo embed --features v2 --target thumbv7em-none-eabihf
  (...)
     Erasing sectors ✔ [00:00:00] [####################################################################################################################################################]  2.00KiB/ 2.00KiB @  4.21KiB/s (eta 0s )
 Programming pages   ✔ [00:00:00] [####################################################################################################################################################]  2.00KiB/ 2.00KiB @  2.71KiB/s (eta 0s )
    Finished flashing in 0.608s

# Pour micro:bit v1
$ cargo embed --features v1 --target thumbv6m-none-eabi
  (...)
     Erasing sectors ✔ [00:00:00] [####################################################################################################################################################]  2.00KiB/ 2.00KiB @  4.14KiB/s (eta 0s )
 Programming pages   ✔ [00:00:00] [####################################################################################################################################################]  2.00KiB/ 2.00KiB @  2.69KiB/s (eta 0s )
    Finished flashing in 0.614s
```


Vous remarquerez que `cargo-embed` se bloque après avoir affiché la dernière ligne, c'est prévu et vous ne devez pas le fermer
puisque nous en avons besoin dans cet état pour l'étape suivante : le débogage ! De plus, vous aurez remarqué que `cargo build`
et `cargo embed` reçoivent en fait les mêmes indicateurs, car `cargo embed` exécute en fait la construction puis
flashe le binaire résultant sur la puce, vous pouvez donc omettre l'étape `cargo build` à l'avenir si vous
souhaitez flasher votre code tout de suite.

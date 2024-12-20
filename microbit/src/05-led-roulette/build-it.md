# Construis-le

La première étape consiste à construire notre crate "binaire". Étant donné que le microcontrôleur a une architecture
différente de celle de votre ordinateur, nous devrons effectuer une compilation croisée. La compilation croisée dans le monde de Rust est aussi simple
que de passer un indicateur `--target` supplémentaire à `rustc` ou Cargo. La partie compliquée consiste à déterminer l'argument
de cet indicateur : le *nom* de la cible.

Comme nous le savons déjà, le microcontrôleur du micro:bit v2 possède un processeur Cortex-M4F, celui du v1 est un Cortex-M0.
`rustc` sait comment effectuer une compilation croisée vers l'architecture Cortex-M et fournit plusieurs cibles différentes qui couvrent les différentes familles de processeurs au sein de cette architecture :

- `thumbv6m-none-eabi`, pour les processeurs Cortex-M0 et Cortex-M1
- `thumbv7m-none-eabi`, pour le processeur Cortex-M3
- `thumbv7em-none-eabi`, pour les processeurs Cortex-M4 et Cortex-M7
- `thumbv7em-none-eabihf`, pour les processeurs Cortex-M4**F** et Cortex-M7**F**
- `thumbv8m.main-none-eabi`, pour les processeurs Cortex-M33 et Cortex-M35P
- `thumbv8m.main-none-eabihf`, pour le Cortex-M33**F** et les processeurs Cortex-M35P**F**

Pour le micro:bit v2, nous utiliserons la cible `thumbv7em-none-eabihf`, pour la v1 celle `thumbv6m-none-eabi`.
Avant de procéder à la compilation croisée, vous devez télécharger une version précompilée de la bibliothèque standard
(une version réduite de celle-ci, en fait) pour votre cible. Cela se fait à l'aide de `rustup` :

``` console
# Pour micro:bit v2
$ rustup target add thumbv7em-none-eabihf
# Pour micro:bit v1
$ rustup target add thumbv6m-none-eabi
```

Vous n'avez besoin d'effectuer l'étape ci-dessus qu'une seule fois ; `rustup` réinstallera une nouvelle bibliothèque standard
(composant `rust-std`) à chaque mise à jour de votre chaîne d'outils. Vous pouvez donc ignorer cette étape si vous avez déjà ajouté la cible
nécessaire lors de la [vérification de votre configuration].

[vérification de votre configuration] : ../03-setup/verify.html#verifying-cargo-embed

Avec le composant `rust-std` en place, vous pouvez maintenant effectuer une compilation croisée du programme à l'aide de Cargo :

``` console
# assurez-vous que vous êtes dans le répertoire `src/05-led-roulette`

# Pour micro:bit v2
$ cargo build --features v2 --target thumbv7em-none-eabihf
   Compiling semver-parser v0.7.0
   Compiling typenum v1.12.0
   Compiling cortex-m v0.6.3
   (...)
   Compiling microbit-v2 v0.10.1
    Finished dev [unoptimized + debuginfo] target(s) in 33.67s

# Pour micro:bit v1
$ cargo build --features v1 --target thumbv6m-none-eabi
   Compiling fixed v1.2.0
   Compiling syn v1.0.39
   Compiling cortex-m v0.6.3
   (...)
   Compiling microbit v0.10.1
	Finished dev [unoptimized + debuginfo] target(s) in 22.73s
```

> **REMARQUE** Assurez-vous de compiler cette caisse *sans* optimisations. Le fichier Cargo.toml
> fourni et la commande de construction ci-dessus garantiront que les optimisations sont désactivées.

OK, nous avons maintenant produit un exécutable. Cet exécutable ne fera clignoter aucune LED,
c'est juste une version simplifiée sur laquelle nous nous baserons plus tard dans le chapitre.
Pour vérifier la cohérence, vérifions que l'exécutable produit est en fait un binaire ARM :

``` console
# Pour micro:bit v2
# équivalent à `readelf -h target/thumbv7em-none-eabihf/debug/led-roulette`
$ cargo readobj --features v2 --target thumbv7em-none-eabihf --bin led-roulette -- --file-headers
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
ELF Header:
  Magic:   7f 45 4c 46 01 01 01 00 00 00 00 00 00 00 00 00
  Class:                             ELF32
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              EXEC (Executable file)
  Machine:                           ARM
  Version:                           0x1
  Entry point address:               0x117
  Start of program headers:          52 (bytes into file)
  Start of section headers:          793112 (bytes into file)
  Flags:                             0x5000400
  Size of this header:               52 (bytes)
  Size of program headers:           32 (bytes)
  Number of program headers:         4
  Size of section headers:           40 (bytes)
  Number of section headers:         21
  Section header string table index: 19

# Pour micro:bit v1
# équivalent à `readelf -h target/thumbv6m-none-eabi/debug/led-roulette`
$ cargo readobj --features v1 --target thumbv6m-none-eabi --bin led-roulette -- --file-headers
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
ELF Header:
  Magic:   7f 45 4c 46 01 01 01 00 00 00 00 00 00 00 00 00
  Class:                             ELF32
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              EXEC (Executable file)
  Machine:                           ARM
  Version:                           0x1
  Entry point address:               0xC1
  Start of program headers:          52 (bytes into file)
  Start of section headers:          693196 (bytes into file)
  Flags:                             0x5000200
  Size of this header:               52 (bytes)
  Size of program headers:           32 (bytes)
  Number of program headers:         4
  Size of section headers:           40 (bytes)
  Number of section headers:         22
  Section header string table index: 20
```

Ensuite, nous allons flasher le programme dans notre microcontrôleur.

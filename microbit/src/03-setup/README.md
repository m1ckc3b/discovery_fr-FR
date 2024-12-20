# Configuration d'un environnement de développement

Travailler avec des microcontrôleurs implique plusieurs outils car nous aurons affaire à une architecture
différente de celle de votre ordinateur et nous devrons exécuter et déboguer des programmes sur un périphérique "distant".

## Documentation

Les outils ne sont cependant pas tout. Sans documentation, il est pratiquement impossible de travailler avec
des microcontrôleurs.

Nous ferons référence à tous ces documents tout au long de ce livre :

- [LSM303AGR]

[LSM303AGR] : https://www.st.com/resource/en/datasheet/lsm303agr.pdf

## Outils

Nous utiliserons tous les outils listés ci-dessous. Lorsqu'une version minimale n'est pas spécifiée, toute version récente
devrait fonctionner, mais nous avons répertorié la version que nous avons testée.

- Rust 1.57.0 ou version plus récente.

- `gdb-multiarch`. Version testée : 10.2. D'autres versions fonctionneront probablement aussi
Si votre distribution/plateforme ne dispose pas de `gdb-multiarch`, `arm-none-eabi-gdb` fera également l'affaire. 
De plus, certains binaires `gdb` normaux sont également construits avec des capacités multiarch, vous pouvez trouver plus d'informations à ce sujet dans les sous-chapitres.

- [`cargo-binutils`]. Version 0.3.3 ou plus récente.

[`cargo-binutils`] : https://github.com/rust-embedded/cargo-binutils

- [`cargo-embed`]. Version 0.24.0 ou plus récente.

[`cargo-embed`] : https://probe.rs/docs/tools/cargo-embed/

- `minicom` sur Linux et macOS. Version testée : 2.7.1. D'autres versions fonctionneront probablement aussi

- `PuTTY` sur Windows.

Ensuite, suivez les instructions d'installation indépendantes du système d'exploitation pour quelques-uns des outils :

### `rustc` & Cargo

Installez rustup en suivant les instructions sur [https://rustup.rs](https://rustup.rs).

Si vous avez déjà installé rustup, vérifiez que vous êtes sur le canal stable
et que votre chaîne d'outils stable est à jour. `rustc -V` doit renvoyer une date
plus récente que celle indiquée ci-dessous :

``` console
$ rustc -V
rustc 1.53.0 (53cb7b09b 2021-06-17)
```

### `cargo-binutils`

``` console
$ rustup component add llvm-tools

$ cargo install cargo-binutils --vers 0.3.3

$ cargo size --version
cargo-size 0.3.3
```

### `cargo-embed`

Pour installer cargo-embed, installez d'abord ses [prérequis](https://probe.rs/docs/getting-started/installation/) (remarque : ces instructions font partie de la boîte à outils de débogage intégrée [`probe-rs`](https://probe.rs/) plus générale). Ensuite, installez-le avec cargo :

```console
$ cargo install --locked probe-rs-tools --vers '^0.24'
```

**REMARQUE** Cela peut échouer en raison de changements fréquents dans `probe-rs`. Si c'est le cas, accédez à <https://probe.rs> et suivez les instructions d'installation actuelles.

Enfin, vérifiez que vous avez correctement installé `cargo-embed` en exécutant :

```console
$ cargo embed --version
cargo-embed 0.24.0 (git commit: crates.io)
```

### Ce référentiel

Étant donné que ce livre contient également quelques petites bases de code Rust utilisées dans divers chapitres
vous devrez également télécharger son code source. Vous pouvez le faire de l'une des manières suivantes :

* Visitez le [dépôt](https://github.com/rust-embedded/discovery/), cliquez sur le bouton vert « Code » puis sur le bouton
« Télécharger le fichier Zip »
* Clonez-le à l'aide de git (si vous connaissez git, vous l'avez probablement déjà installé) à partir du même dépôt que celui lié dans
l'approche zip

### Instructions spécifiques au système d'exploitation

Suivez maintenant les instructions spécifiques au système d'exploitation que vous utilisez :

- [Linux](linux.md)
- [Windows](windows.md)
- [macOS](macos.md)
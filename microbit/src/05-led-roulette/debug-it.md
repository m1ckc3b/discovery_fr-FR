# Débogue-le

## Comment cela fonctionne-t-il ?
Avant de déboguer notre petit programme, prenons un moment pour comprendre rapidement ce qui se passe
réellement ici. Dans le chapitre précédent, nous avons déjà discuté de l'objectif de la deuxième puce
sur la carte ainsi que de la façon dont elle communique avec notre ordinateur, mais comment pouvons-nous réellement l'utiliser ?

La petite option `default.gdb.enabled = true` dans `Embed.toml` a fait ouvrir à `cargo-embed` un soi-disant "stub GDB" après le flashage,
il s'agit d'un serveur auquel notre GDB peut se connecter et envoyer des commandes telles que "définir un point d'arrêt à l'adresse X". Le serveur peut alors décider de lui-même comment gérer cette commande. Dans le cas du stub GDB `cargo-embed`, il transmettra la commande à la sonde de débogage sur la carte via USB qui se chargera ensuite de communiquer avec le MCU pour nous.

## Déboguons !

Étant donné que `cargo-embed` bloque notre shell actuel, nous pouvons simplement en ouvrir un nouveau et revenir dans notre répertoire de projet.
Une fois que nous y sommes, nous devons d'abord ouvrir le binaire dans gdb comme ceci :

```shell
# Pour micro:bit v2
$ gdb target/thumbv7em-none-eabihf/debug/led-roulette

# Pour micro:bit v1
$ gdb target/thumbv6m-none-eabi/debug/led-roulette
```

> **REMARQUE** : selon le GDB que vous avez installé, vous devrez utiliser une commande différente pour le lancer,
> consultez le [chapitre 3] si vous avez oublié de laquelle il s'agissait.

[chapitre 3] : ../03-setup/index.md#tools

> **REMARQUE** : si vous obtenez l'erreur `target/thumbv7em-none-eabihf/debug/led-roulette : No such file or directory`
>, essayez d'ajouter `../../` au chemin du fichier, par exemple :

```shell
$ gdb ../../target/thumbv7em-none-eabihf/debug/led-roulette
 ```

Cela est dû au fait que chaque exemple de projet se trouve dans un `espace de travail` qui contient l'intégralité du livre, et que les espaces de travail ont un seul répertoire `target`. Consultez le [chapitre Espaces de travail dans le livre Rust] pour en savoir plus.

[Chapitre Espaces de travail dans le livre Rust] : https://doc.rust-lang.org/book/ch14-03-cargo-workspaces.html#creating-a-workspace

> **REMARQUE** : si `cargo-embed` affiche beaucoup d'avertissements ici, ne vous inquiétez pas. Pour l'instant, il n'implémente pas complètement le protocole > GDB et peut donc ne pas reconnaître toutes les commandes que votre GDB lui envoie, tant qu'il ne plante pas, tout va bien.

Ensuite, nous devrons nous connecter au stub GDB. Il s'exécute sur `localhost:1337` par défaut, donc pour vous y connecter, exécutez ce qui suit :

```shell
(gdb) target remote :1337
Remote debugging using :1337
0x00000116 in nrf52833_pac::{{impl}}::fmt (self=0xd472e165, f=0x3c195ff7) at /home/nix/.cargo/registry/src/github.com-1ecc6299db9ec823/nrf52833-pac-0.9.0/src/lib.rs:157
157     #[derive(Copy, Clone, Debug)]
```

Ensuite, nous voulons accéder à la fonction principale de notre programme.
Nous allons le faire en définissant d'abord un point d'arrêt et en poursuivant
l'exécution du programme jusqu'à ce que nous atteignions le point d'arrêt :

```
(gdb) break main
Breakpoint 1 at 0x104: file src/05-led-roulette/src/main.rs, line 9.
Note: automatically using hardware breakpoints for read-only addresses.
(gdb) continue
Continuing.

Breakpoint 1, led_roulette::__cortex_m_rt_main_trampoline () at src/05-led-roulette/src/main.rs:9
9       #[entry]
```

Les points d'arrêt peuvent être utilisés pour arrêter le déroulement normal d'un programme. La commande `continue` permettra au
programme de s'exécuter librement *jusqu'à* qu'il atteigne un point d'arrêt. Dans ce cas, jusqu'à ce qu'il atteigne la
fonction `main` car il y a un point d'arrêt à cet endroit.

Notez que la sortie GDB indique "Point d'arrêt 1". N'oubliez pas que notre processeur ne peut utiliser qu'une quantité limitée de ces
points d'arrêt, il est donc judicieux de prêter attention à ces messages. Si vous n'avez plus de points d'arrêt,
vous pouvez lister tous les points d'arrêt actuels avec `info break` et supprimer ceux souhaités avec `delete <breakpoint-num>`.

Pour une expérience de débogage plus agréable, nous utiliserons l'interface utilisateur texte (TUI) de GDB. Pour entrer dans ce
mode, sur le shell GDB, entrez la commande suivante :

```
(gdb) layout src
```

> **REMARQUE** : nos excuses aux utilisateurs de Windows. Le GDB fourni avec la chaîne d'outils intégrée GNU ARM ne
> prend pas en charge ce mode TUI `:-(`.

![Session GDB](../assets/gdb-layout-src.png "GDB TUI")

La commande break de GDB ne fonctionne pas seulement pour les noms de fonctions, elle peut également s'interrompre à certains numéros de ligne.
Si nous voulions faire une interruption à la ligne 13, nous pouvons simplement faire :

```
(gdb) break 13
Breakpoint 2 at 0x110: file src/05-led-roulette/src/main.rs, line 13.
(gdb) continue
Continuing.

Breakpoint 2, led_roulette::__cortex_m_rt_main () at src/05-led-roulette/src/main.rs:13
(gdb)
```

À tout moment, vous pouvez quitter le mode TUI en utilisant la commande suivante :

```
(gdb) tui disable
```

Nous sommes maintenant sur l'instruction `_y = x` ; cette instruction n'a pas encore été exécutée. Cela signifie que `x`
est initialisé mais pas `_y`. Inspectons ces variables de pile/locales à l'aide de la commande `print` :

```
(gdb) print x
$1 = 42
(gdb) print &x
$2 = (*mut i32) 0x20003fe8
(gdb)
```

Comme prévu, `x` contient la valeur `42`. La commande `print &x` imprime l'adresse de la variable `x`.
Ce qui est intéressant ici, c'est que la sortie GDB affiche le type de la référence : `i32*`, un pointeur vers une valeur `i32`.

Si nous voulons continuer l'exécution du programme ligne par ligne, nous pouvons le faire en utilisant la commande `next`
alors passons à l'instruction `loop {}` :

```
(gdb) next
16          loop {}
```

Et `_y` devrait maintenant être initialisé.

```
(gdb) print _y
$5 = 42
```

Au lieu d'imprimer les variables locales une par une, vous pouvez également utiliser la commande `info locals` :

```
(gdb) info locals
x = 42
_y = 42
(gdb)
```

Si nous utilisons à nouveau `next` au-dessus de l'instruction `loop {}`, nous serons bloqués car le programme
ne transmettra jamais cette instruction. Au lieu de cela, nous passerons à la vue de désassemblage avec la commande `layout asm`
et avancerons d'une instruction à la fois en utilisant `stepi`. Vous pouvez toujours revenir à la vue du code source Rust
plus tard en exécutant à nouveau la commande `layout src`.

> **REMARQUE** : si vous avez utilisé la commande `next` ou `continue` par erreur et que GDB est bloqué, vous pouvez vous débloquer en appuyant sur `Ctrl+C`.

```
(gdb) layout asm
```

![Session GDB](../assets/gdb-layout-asm.png "Désassemblage GDB")

Si vous n'utilisez pas le mode TUI, vous pouvez utiliser la commande `disassemble /m` pour désassembler le
programme autour de la ligne sur laquelle vous vous trouvez actuellement.

```
(gdb) disassemble /m
Dump of assembler code for function _ZN12led_roulette18__cortex_m_rt_main17h3e25e3afbec4e196E:
10      fn main() -> ! {
   0x0000010a <+0>:     sub     sp, #8
   0x0000010c <+2>:     movs    r0, #42 ; 0x2a

11          let _y;
12          let x = 42;
   0x0000010e <+4>:     str     r0, [sp, #0]

13          _y = x;
   0x00000110 <+6>:     str     r0, [sp, #4]

14
15          // infinite loop; just so we don't leave this stack frame
16          loop {}
=> 0x00000112 <+8>:     b.n     0x114 <_ZN12led_roulette18__cortex_m_rt_main17h3e25e3afbec4e196E+10>
   0x00000114 <+10>:    b.n     0x114 <_ZN12led_roulette18__cortex_m_rt_main17h3e25e3afbec4e196E+10>

End of assembler dump.
```

Vous voyez la grosse flèche `=>` sur le côté gauche ? Elle indique l'instruction que le processeur exécutera ensuite.

Si vous n'êtes pas dans le mode TUI, à chaque commande `stepi`, GDB imprimera l'instruction et le numéro de ligne
de l'instruction que le processeur exécutera ensuite.

```
(gdb) stepi
16          loop {}
(gdb) stepi
16          loop {}
```

Une dernière astuce avant de passer à quelque chose de plus intéressant. Entrez les commandes suivantes dans GDB :

```
(gdb) monitor reset
(gdb) c
Continuing.

Breakpoint 1, led_roulette::__cortex_m_rt_main_trampoline () at src/05-led-roulette/src/main.rs:9
9       #[entry]
(gdb)
```

Nous revenons maintenant au début de `main` !

`monitor reset` réinitialisera le microcontrôleur et l'arrêtera juste au point d'entrée du programme.

La commande `continue` suivante permettra au programme de s'exécuter librement jusqu'à ce qu'il atteigne la fonction `main`
qui comporte un point d'arrêt.

Cette combinaison est pratique lorsque vous avez, par erreur, ignoré une partie du programme que vous souhaitiez
inspecter. Vous pouvez facilement ramener l'état de votre programme à son
début.

> **Les petits caractères** : Cette commande `reset` n'efface ni ne touche la RAM. Cette mémoire conservera ses
> valeurs de l'exécution précédente. Cela ne devrait cependant pas poser de problème, à moins que le comportement de votre programme
> ne dépende de la valeur des variables *non initialisées*, mais c'est la définition du comportement
> indéfini (UB).

Nous en avons terminé avec cette session de débogage. Vous pouvez la terminer avec la commande `quit`.

```
(gdb) quit
A debugging session is active.

        Inferior 1 [Remote target] will be detached.

Quit anyway? (y or n) y
Detaching from program: $PWD/target/thumbv7em-none-eabihf/debug/led-roulette, Remote target
Ending remote debugging.
[Inferior 1 (Remote target) detached]
```

> **REMARQUE** : si la CLI GDB par défaut ne vous convient pas, consultez [gdb-dashboard]. Il utilise Python pour
> transformer la CLI GDB par défaut en un tableau de bord qui affiche les registres, la vue source, la vue d'assemblage
> et d'autres choses.

[gdb-dashboard] : https://github.com/cyrus-and/gdb-dashboard#gdb-dashboard

Si vous souhaitez en savoir plus sur ce que GDB peut faire, consultez la section [Comment utiliser GDB](../appendix/2-how-to-use-gdb/).

Et ensuite ? L'API de haut niveau que j'ai promise.

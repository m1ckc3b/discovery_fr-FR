# Contexte

## Qu'est-ce qu'un microcontrôleur ?

Un microcontrôleur est un *système* sur une puce. Alors que votre ordinateur est composé de plusieurs
composants discrets : un processeur, une RAM, un stockage, un port Ethernet, etc. ; un microcontrôleur possède tous ces types
de composants intégrés dans une seule "puce" ou un seul boîtier. Cela permet de construire des systèmes avec
moins de pièces.

## Que peut-on faire avec un microcontrôleur ?

Beaucoup de choses ! Les microcontrôleurs sont la partie centrale de ce que l'on appelle les "systèmes *embarqués*".
Les systèmes embarqués sont partout, mais vous ne les remarquez généralement pas. Ils contrôlent les machines qui
lavent vos vêtements, impriment vos documents et cuisinent vos aliments. Les systèmes embarqués maintiennent les bâtiments
dans lesquels vous vivez et travaillez à une température confortable et contrôlent les composants qui font que
les véhicules dans lesquels vous voyagez s'arrêtent et repartent.

La plupart des systèmes embarqués fonctionnent sans intervention de l'utilisateur. Même s'ils exposent une interface utilisateur comme le fait
une machine à laver ; la plupart de leur fonctionnement est effectué de manière autonome.

Les systèmes embarqués sont souvent utilisés pour *contrôler* un processus physique. Pour rendre cela possible, ils disposent
d'un ou plusieurs dispositifs pour les informer de l'état du monde (« capteurs ») et d'un ou plusieurs
dispositifs qui leur permettent de changer les choses (« actionneurs »). Par exemple, un système de contrôle de la climatisation d'un bâtiment
peut avoir :

- Des capteurs qui mesurent la température et l'humidité à divers endroits.

- Des actionneurs qui contrôlent la vitesse des ventilateurs.

- Des actionneurs qui provoquent l'ajout ou le retrait de chaleur du bâtiment.

## Quand dois-je utiliser un microcontrôleur ?

La plupart des systèmes embarqués énumérés ci-dessus pourraient être mis en œuvre avec un ordinateur exécutant Linux (par
exemple un « Raspberry Pi »). Pourquoi utiliser un microcontrôleur à la place ? Il semble qu'il soit plus difficile de
développer un programme.

Voici quelques raisons :

**Coût.** Un microcontrôleur est beaucoup moins cher qu'un ordinateur à usage général. Non seulement le
microcontrôleur est moins cher, mais il nécessite également beaucoup moins de composants électriques externes pour fonctionner.
Cela rend les cartes de circuits imprimés (PCB) plus petites et moins chères à concevoir et à fabriquer.

**Consommation d'énergie.** La plupart des microcontrôleurs consomment une fraction de l'énergie d'un
processeur complet. Pour les applications fonctionnant sur batterie, cela fait une énorme différence.

**Réactivité.** Pour atteindre leur objectif, certains systèmes embarqués doivent toujours réagir dans un
intervalle de temps limité (par exemple, le système de freinage "antiblocage" d'une voiture). Si le système ne respecte pas ce
type de *délai*, une panne catastrophique peut se produire. Un tel délai est appelé une exigence
"temps réel dur". Un système embarqué qui est lié par un tel délai est appelé "système temps réel dur". Un ordinateur à usage général et un système d'exploitation comportent généralement de nombreux
composants logiciels qui partagent les ressources de traitement de l'ordinateur. Il est donc plus difficile de garantir l'exécution d'un
programme dans des délais serrés.

**Fiabilité.** Dans les systèmes comportant moins de composants (matériels et logiciels), il y a moins de risques de
défaillance !

## Quand dois-je *ne pas* utiliser un microcontrôleur ?

Lorsque des calculs lourds sont impliqués. Pour maintenir leur consommation d'énergie à un niveau bas, les microcontrôleurs ont
des ressources de calcul très limitées à leur disposition. Par exemple, certains microcontrôleurs n'ont même pas
de support matériel pour les opérations en virgule flottante. Sur ces appareils, effectuer une simple
addition de nombres en simple précision peut prendre des centaines de cycles CPU.

## Pourquoi utiliser Rust et pas C ?

J'espère que je n'ai pas besoin de vous convaincre ici, car vous connaissez probablement les
différences de langage entre Rust et C. Un point que je veux soulever est la gestion des paquets. C manque d'une
solution de gestion de paquets officielle et largement acceptée alors que Rust a Cargo. Cela rend le développement
*beaucoup* plus facile. Et, à mon avis, une gestion facile des paquets encourage la réutilisation du code car les bibliothèques peuvent être
facilement intégrées dans une application, ce qui est également une bonne chose car les bibliothèques font l'objet de plus de "tests de combat".

## Pourquoi ne devrais-je pas utiliser Rust ?

Ou pourquoi devrais-je préférer C à Rust ?

L'écosystème C est bien plus mature. Il existe déjà des solutions prêtes à l'emploi pour plusieurs problèmes. Si vous devez contrôler un processus sensible au temps, vous pouvez vous procurer l'un des systèmes d'exploitation en temps réel (RTOS) commerciaux existants et résoudre votre problème. Il n'existe pas encore de RTOS commercial de niveau production dans Rust, vous devrez donc en créer un vous-même ou essayer l'un de ceux qui sont en cours de développement. Vous pouvez trouver une liste de ceux-ci dans le dépôt [Awesome Embedded Rust].

[Awesome Embedded Rust] : https://github.com/rust-embedded/awesome-embedded-rust#real-time-operating-system-rtos
# Découverte

> Découvrez le monde des microcontrolleurs grâce à [Rust]!

[Rust]: https://www.rust-lang.org/fr/

Ce livre est un cours d'introduction aux systèmes embarqués basés sur des microcontrôleurs qui utilise Rust comme
langage d'enseignement plutôt que le C/C++ habituel.

## Portée

Les sujets suivants seront abordés (éventuellement, je l'espère) :

- Comment écrire, construire, flasher et déboguer un programme " embarqué " (Rust).

- Fonctionnalités (" périphériques ") que l'on trouve couramment dans les microcontrôleurs : entrée et sortie numériques, modulation de largeur d'impulsion (PWM), convertisseurs analogique-numérique (ADC), protocoles de communication courants tels que
série, I2C et SPI, etc.

- Concepts multitâches : multitâche coopératif ou préemptif, interruptions, planificateurs, etc.

- Concepts des systèmes de contrôle : capteurs, étalonnage, filtres numériques, actionneurs, contrôle en boucle ouverte,
contrôle en boucle fermée, etc.

## Approche

- Adapté aux débutants. Aucune expérience préalable avec les microcontrôleurs ou les systèmes embarqués n'est requise.

- Pratique. De nombreux exercices pour mettre la théorie en pratique. *Vous* ferez la plupart du travail ici.

- Centré sur les outils. Nous utiliserons largement les outils pour faciliter le développement. Le débogage "réel", avec GDB,
et la journalisation seront introduits dès le début. L'utilisation de LED comme mécanisme de débogage n'a pas sa place ici.

## Non-objectifs

Ce qui est hors de portée de ce livre :

- Enseigner Rust. Il existe déjà beaucoup de matériel sur ce sujet. Nous nous concentrerons sur les microcontrôleurs
et les systèmes embarqués.

- Être un texte complet sur la théorie des circuits électriques ou l'électronique. Nous couvrirons simplement le
minimum requis pour comprendre le fonctionnement de certains appareils.

- Couvrir des détails tels que les scripts de liaison et le processus de démarrage. Par exemple, nous utiliserons des outils
existants pour vous aider à placer votre code sur votre carte, mais nous n'entrerons pas dans les détails sur le fonctionnement de ces outils.

Je n'ai pas non plus l'intention de porter ce matériel sur d'autres cartes de développement ; ce livre fera un usage exclusif
de la carte de développement micro:bit.

## Signaler des problèmes

La source de ce livre se trouve dans [ce dépôt]. Si vous rencontrez une faute de frappe ou un problème avec le code
signalez-le sur le [suivi des problèmes].

[ce référentiel] : https://github.com/rust-embedded/discovery
[suivi des problèmes] : https://github.com/rust-embedded/discovery/issues

## Autres ressources Rust intégrées

Ce livre Discovery n'est qu'une des nombreuses ressources Rust intégrées fournies par le
[Groupe de travail intégré]. La sélection complète est disponible dans [The Embedded Rust Bookshelf]. Cela
comprend la liste des [Questions fréquemment posées].

[Groupe de travail intégré] : https://github.com/rust-embedded/wg
[La bibliothèque Embedded Rust] : https://docs.rust-embedded.org
[Foire aux questions] : https://docs.rust-embedded.org/faq.html

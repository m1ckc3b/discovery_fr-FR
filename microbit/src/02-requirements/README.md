# Exigences matérielles/connaissances

La principale exigence de connaissances pour lire ce livre est de connaître *un peu* de Rust. Il est
difficile pour moi de quantifier *un peu* mais au moins je peux vous dire que vous n'avez pas besoin
de comprendre complètement les génériques, mais vous devez savoir comment *utiliser* les fermetures. Vous devez également
être familier avec les idiomes de l'[édition 2018], en particulier
avec le fait que `extern crate` n'est pas nécessaire dans l'édition 2018.

[édition 2018] : https://rust-lang-nursery.github.io/edition-guide/

De plus, pour suivre ce matériel, vous aurez besoin du matériel suivant :

- Une carte [micro:bit v2], ou bien une carte [micro:bit v1.5], le livre
fera référence à la v1.5 comme étant simplement v1.

[micro:bit v2] : https://tech.microbit.org/hardware/
[micro:bit v1.5] : https://tech.microbit.org/hardware/1-5-revision/

(Vous pouvez acheter cette carte auprès de plusieurs [fournisseurs][0][1] d'électronique)

[0] : https://microbit.org/buy/
[1] : https://www.mouser.com/microbit/_/N-aez3t?P=1y8um0l

<p align="center">
<img title="micro:bit" src="../assets/microbit-v2.jpg">
</p>

> **REMARQUE** Ceci est une image d'un micro:bit v2, l'avant du v1 est légèrement différent

- Un câble USB micro-B, nécessaire pour faire fonctionner la carte micro:bit.
Assurez-vous que le câble prend en charge le transfert de données, car certains câbles ne prennent en charge que les appareils de charge.

<p align="center">
<img title="câble USB micro-B" src="../assets/usb-cable.jpg">
</p>

> **REMARQUE** Vous possédez peut-être déjà un câble comme celui-ci, car certains kits micro:bit sont livrés avec de tels câbles.
> Certains câbles USB utilisés pour charger des appareils mobiles peuvent également fonctionner, s'ils sont micro-B et ont la
> capacité de transmettre des données.

> **FAQ** : Attendez, pourquoi ai-je besoin de ce matériel spécifique ?

Cela facilite grandement ma vie et la vôtre.

Le matériel est beaucoup, beaucoup plus accessible si nous n'avons pas à nous soucier des différences matérielles.
Faites-moi confiance sur ce point.

> **FAQ** : Puis-je suivre ce matériel avec une autre carte de développement ?

Peut-être ? Cela dépend principalement de deux choses : votre expérience antérieure avec les microcontrôleurs et/ou
si une caisse de haut niveau existe déjà, comme le [`nrf52-hal`], pour votre carte de développement
quelque part. Vous pouvez consulter la [Awesome Embedded Rust HAL list] pour votre microcontrôleur,
si vous avez l'intention d'en utiliser un autre.

[`nrf52-hal`] : https://docs.rs/nrf52-hal
[Awesome Embedded Rust HAL list] : https://github.com/rust-embedded/awesome-embedded-rust#hal-implementation-crates

Avec une autre carte de développement, ce texte perdrait la plupart, voire la totalité, de sa convivialité pour les débutants
et de son côté « facile à suivre », à mon avis.

Si vous avez une autre carte de développement et que vous ne vous considérez pas comme un débutant total, il est
mieux de commencer avec le modèle de projet [quickstart].

[quickstart] : https://rust-embedded.github.io/cortex-m-quickstart/cortex_m_quickstart/
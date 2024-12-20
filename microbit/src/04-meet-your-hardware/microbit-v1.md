# Nordic nRF51822 (le « nRF51 », micro:bit v1)

Notre MCU possède 48 minuscules **broches** métalliques situées juste en dessous (c'est une puce dite [QFN48]).
Ces broches sont connectées à des **traces**, les petites « routes » qui agissent comme les fils reliant les composants
entre eux sur la carte. Le MCU peut modifier de manière dynamique les propriétés électriques
des broches. Cela fonctionne de manière similaire à un interrupteur d'éclairage qui modifie la
façon dont le courant électrique circule dans un circuit. En activant ou en désactivant le courant électrique
qui circule dans une broche spécifique, une LED attachée à cette broche (via les traces)
peut être allumée et éteinte.

Chaque fabricant utilise un système de numérotation de pièces différent, mais beaucoup vous permettront
de déterminer des informations sur un composant simplement en regardant le numéro de pièce. En regardant le numéro de référence de notre MCU (`N51822 QFAAH3 1951LN`, ​​vous ne pouvez probablement pas
le voir à l'œil nu, mais il est sur la puce), le `n` à l'avant
nous indique qu'il s'agit d'une pièce fabriquée par [Nordic Semiconductor].
En recherchant le numéro de référence sur leur site Web, nous trouvons rapidement la [page produit].
Nous y apprenons que le principal argument marketing de notre puce est qu'il s'agit d'un
"SoC Bluetooth Low Energy et 2,4 GHz" (SoC étant l'abréviation de "System on a Chip"),
ce qui explique la présence de RF dans le nom du produit puisque RF est l'abréviation de radiofréquence.
Si nous parcourons un peu la documentation de la puce liée sur la [page produit]
nous trouvons la [spécification produit] qui contient le chapitre 10 "Informations de commande"
consacré à l'explication de l'étrange dénomination de la puce. Ici, nous apprenons que :

[QFN48] : https://fr.wikipedia.org/wiki/Quad_Flat_No-leads_package
[Nordic Semiconductor] : https://www.nordicsemi.com/
[page produit] : https://www.nordicsemi.com/products/nrf51822
[spécification produit] : https://infocenter.nordicsemi.com/pdf/nRF51822_PS_v3.3.pdf

- Le « N51 » est la série du MCU, indiquant qu'il existe d'autres MCU « nRF51 »
- Le « 822 » est le code de la pièce
- Le « QF » est le code du package, dans ce cas, l'abréviation de « QFN48 »
- Le « AA » est le code de la variante, indiquant la quantité de RAM et de mémoire flash dont dispose le MCU,
dans notre cas, 256 kilo-octets de mémoire flash et 16 kilo-octets de RAM
- Le `H3` est le code de construction, indiquant la version matérielle (`H`) ainsi que la configuration du produit (`3`)
- Le `1951LN` est un code de suivi, il peut donc différer sur votre puce

La spécification du produit contient bien sûr beaucoup plus d'informations utiles sur
la puce, par exemple qu'elle est basée sur un processeur ARM® Cortex™-M0 32 bits.

### Arm ? Cortex-M0 ?

Si notre puce est fabriquée par Nordic, alors qui est Arm ? Et si notre puce est la
nRF51822, qu'est-ce que le Cortex-M0 ?

Vous serez peut-être surpris d'apprendre que même si les puces "basées sur Arm" sont
assez populaires, la société derrière la marque "Arm" ([Arm Holdings]) ne
fabrique pas réellement de puces destinées à l'achat. Au lieu de cela, leur principal modèle commercial
est simplement de *concevoir* des pièces de puces. Ils concèderont ensuite ces conceptions sous licence à des
fabricants, qui à leur tour les mettront en œuvre (peut-être avec quelques-unes de leurs
propres modifications) sous la forme de matériel physique qui pourra ensuite être vendu.
La stratégie d'Arm ici est différente de celle d'entreprises comme Intel, qui
conçoit *et* fabrique ses puces.

Arm octroie des licences à un tas de conceptions différentes. Leur famille de conceptions "Cortex-M"
est principalement utilisée comme cœur dans les microcontrôleurs. Par exemple, le Cortex-M0
(le cœur sur lequel notre puce est basée) est conçu pour un faible coût et une faible consommation d'énergie.
Le Cortex-M7 est plus cher, mais offre plus de fonctionnalités et de performances.

Heureusement, vous n'avez pas besoin d'en savoir trop sur les différents types de processeurs
ou de conceptions Cortex pour les besoins de ce livre. Cependant, j'espère que vous êtes maintenant un peu
plus informé sur la terminologie de votre appareil. Lorsque vous
travaillez spécifiquement avec un nRF51822, vous pourriez vous retrouver à lire
la documentation et à utiliser des outils pour les puces basées sur Cortex-M, car le nRF51822
est basé sur une conception Cortex-M.

[Arm Holdings] : https://www.arm.com/
# Tirer le meilleur parti de votre IDE

Tout le code de ce livre suppose que vous utilisez un terminal simple pour créer votre code,
l'exécuter et interagir avec lui. Il ne fait également aucune supposition sur votre éditeur de texte.

Cependant, vous pouvez avoir vos IDE préférés, vous offrant la saisie semi-automatique, l'annotation de type,
vos raccourcis préférés et bien plus encore. Cette section explique comment tirer le meilleur parti
de votre IDE en utilisant le code obtenu à partir du dépôt de ce livre.

# Complétion automatique, annotation de type et plus encore

Certains IDE ne parviennent pas à comprendre le code, car ils ne parviennent pas à déterminer si un terme
est défini dans la base de code microbit ou microbit-v2. Si vous ne parvenez pas à faire fonctionner la saisie semi-automatique,
vous pouvez essayer de modifier les fichiers `Cargo.toml` que vous rencontrez dans ce livre et supprimer
toutes les références à la version de microbit que vous n'utilisez pas. C'est-à-dire :
dans le fichier `Cargo.toml`, vous devez supprimer la dépendance et les fonctionnalités que vous n'utilisez pas (la partie protégée par `#[cfg(feature = "vI")]` et la protection elle-même)

# Configuration de l'IDE

Ci-dessous, nous expliquons comment configurer votre IDE pour tirer le meilleur parti de ce livre.
Si votre IDE n'est pas répertorié ci-dessous, veuillez améliorer ce livre en ajoutant une section, afin que le prochain
lecteur puisse en tirer la meilleure expérience.

## Comment construire avec IntelliJ

Lors de la modification de la configuration de construction d'IntelliJ, voici quelques valeurs non par défaut :
* Vous devez modifier la commande. Lorsque ce livre vous indique d'exécuter `cargo embed FLAGS`,
vous devrez remplacer la valeur par défaut `run` par la commande `embed FLAGS`,
* Vous devez activer « Émuler le terminal dans la console de sortie ». Sinon, votre programme ne parviendra pas à imprimer le texte sur un terminal
* Vous devez vous assurer que le répertoire de travail est `microbit/src/N-name`, `N-name` étant le répertoire du chapitre que vous
êtes en train de lire. Vous ne pouvez pas exécuter à partir du répertoire `src` car il ne contient aucun fichier cargo.
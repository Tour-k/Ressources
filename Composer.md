# Composer

Composer est un gestionnaire de dépendances libre, écrit en PHP. Il permet à ses utilisateurs de déclarer et d'installer les bibliothèques dont le projet principal a besoin.

## Installation

Il est possible d'installer Composer directement dans ton projet sous la forme d'un fichier composer.phar (PHP Archive) ou de manière globale à ton système.

Pour commencer telecharge ces fichiers : https://getcomposer.org/installer
Une fois installé, lance la commande suivante pour installer Composer de manière globale sur ton système :
```cli
mv composer.phar /usr/local/bin/composer
```

Ensuite il faut verifier si composer est bien à jour :
````cli
composer self-update
````
Autre méthode si la première ne fonctionne pas ; lance la commande suivante pour installer Composer

    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"

    php -r "if (hash_file('sha384', 'composer-setup.php') === '48e3236262b34d30969dca3c37281b3b4bbe3221bda826ac6a9a62d6444cdb0dcd0615698a5cbe587c3f0fe57a54d8f5') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"

    php composer-setup.php

    php -r "unlink('composer-setup.php');"



## Initialisation d'un projet composer

Afin d'initialiser un projet qui va utiliser Composer, lance la commande (toujours à la racine de ton projet) :
````cli
composer init
````

Cette commande est interactive. Elle va donc te demander de renseigner des informations spécifiques à ton projet.
Une fois que tu as répondu à toutes les questions, Composer t'a créé un fichier très important :

composer.json

Ce fichier contient toutes les informations relatives à ton projet que tu as renseignées dans ton terminal. Il contiendra également la liste des dépendances à gérer.

Voici le contenu de ce fichier :
````json
{
    "name": "wcs/quete_composer",
    "description": "Projet quete composer",
    "authors": [
        {
            "name": "WCS",
            "email": "formateur@wildcodeschool.fr"
        }
    ],
    "require": {}
}
````
Pour le moment il n'y a aucune dépendance. Les dépendances seront enregistrées dans la clé require.

## Chargement des fichiers

La différence entre require* et include* est le niveau d'erreur. Les require* affichent une ERROR en cas de fichier inexistant, les include* affichent un WARNING. Le once s'assure que le fichier n'est chargé qu'une seule fois.

Cette méthode fonctionne parfaitement mais est vite assez lourde quand il y a beaucoup de fichiers à charger.

## Chargement des classes

Tes classes étant définies dans des fichiers PHP (1 fichier par classe), et "rangées" dans des namespaces bien ordonnés, lorsque tu vas vouloir instancier un nouvel objet, PHP doit avoir déjà lu le fichier contenant la classe. Pour cela, tu dois faire des require vers les fichiers .php contenant les classes que tu souhaites utiliser.

Mais comme pour les fichiers, tu vas utiliser beaucoup de classes et cela va vite devenir ingérable ! C'est pour cela que PHP propose un système d'autoloading. Mais tu ne vas pas l'utiliser directement, car il faut le paramétrer et cela peut vite devenir compliqué. Tu vas donc utiliser le système d'autoloading de Composer.

Un petit tuto Grafikart : https://www.grafikart.fr/tutoriels/autoload-561

Pour autoloader les classes  il faut ajouter dans le fichier composer.json
````json
{
    "autoload": {
        "psr-4": {"Acme\\": "src/"}
    }
}
````
Dans psr-4, la première partie correspond au namespace à charger et la seconde partie correspond au path où se trouve le dossier contenant les classes par rapport à l'index, sur lequel on va mettre le require vendor/autoload.
Un petit tuto à suivre pour mettre en place l'autoload :
https://jeanbaptistemarie.com/notes/code/composer/php-composer-autoload.html

## Dépendances

Mais qu'est-ce qu'une dépendance ? Une dépendance est une librairie développée par un tiers mais que tu vas utiliser dans ton projet.

En effet, les problématiques rencontrées sont souvent les mêmes, et il existe déjà des librairies, souvent Open Source (mais pas forcément), qui résolvent ces problèmes. De plus, ces librairies ont souvent toute une communauté de développeurs et d'utilisateurs qui les mettent à jour et les font évoluer.

Tu ne peux pas installer n'importe quelle librairie via Composer. Toutes les librairies disponibles à l'installation sont présentes sur le site https://packagist.org.

Packagist fournit des informations sur :

- Le nombre de téléchargements
- Le nombre de projets dépendants
- Le Github du projet
- Les versions
- Les dépendances de la dépendance XD
- ...

### Installer une dépendance

Chercher sur le site https://packagist.org le nom de la dépendance puis lancer dans le terminal à la racine de ton projet la commande :
````cli
composer require swiftmailer/swiftmailer
````
Ici pour installer la dépendance swifmailer (Pour envoyer des mails ;) )

À la fin de l'installation, il est important de noter deux choses inscrites dans le terminal :
```
Writing lock file (cf. section lock file)
Generating autoload files (cf. section composer autoloading)
```
Il faut aussi noter que ton fichier composer.json a été mis à jour. Il contient maintenant ceci :
```json
{
    "name": "wcs/quete_composer",
    "description": "Projet quete composer",
    "authors": [
        {
            "name": "WCS",
            "email": "formateur@wildcodeschool.fr"
        }
    ],
    "require": {
        "swiftmailer/swiftmailer": "^6.0"
    }
}
```
La librairie SwiftMailer a été ajoutée dans la section require.

## Lock file

Le fichier composer.lock est directement lié au fichier composer.json que tu as vu plus haut. À la fin de l'installation d'une librairie, Composer t'indique Writing lock file. Cela signifie que Composer va soit créer le fichier composer.lock, soit le mettre à jour s'il existe déjà.

compose.lock VS composer.json

Le fichier composer.json contient des informations sur le projet, la liste des dépendances qu'il utilise, les versions de ces dépendances ainsi que les règles pour les mettre à jour.

Quant au fichier composer.lock, il contient la liste des dépendances ainsi que les versions précises installées.

Cela peut sembler une petite différence mais c'est en fait très différent car chaque mise à jour d'une dépendance peut entraîner un bug ou une incompatibilité.

Tu ne versionnes si possible que le composer.lock pour éviter que, par accident, un développeur n'ait pas la même version de ses dépendances que les autres.

### install VS update

De la même manière qu'il existe deux fichiers composer.lock et composer.json, il existe deux commandes pour les manipuler.
```cli
composer update

// OU

composer install
```
Contrairement à ce que l'on pourrait croire, la commande ```install``` n'est pas utilisée uniquement lors de l'installation, mais également pour les mises à jour. Cette commande va lire directement le fichier ```composer.lock``` et installer les versions précises décrites dedans.

La commande ```update``` va, elle, lire le fichier composer.json et mettre à jour si nécessaire les dépendances en fonction des règles écrites.

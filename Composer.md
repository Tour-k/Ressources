# Composer 

Composer est un gestionnaire de dépendances libre, écrit en PHP. Il permet à ses utilisateurs de déclarer et d'installer les bibliothèques dont le projet principal a besoin.

## Installation 

Il est possible d'installer Composer directement dans ton projet sous la forme d'un fichier composer.phar (PHP Archive) ou de manière globale à ton système.

Pour commencer telecharge ces fichiers : https://getcomposer.org/installer
Une fois installé, lance la commande suivante pour installer Composer de manière globale sur ton système : 
```
mv composer.phar /usr/local/bin/composer
```

Ensuite il faut verifier si composer est bien à jour : 
````
composer self-update
````

## Initialisation d'un projet composer 

Afin d'initialiser un projet qui va utiliser Composer, lance la commande (toujours à la racine de ton projet) :
````
composer init
````

Cette commande est interactive. Elle va donc te demander de renseigner des informations spécifiques à ton projet.
Une fois que tu as répondu à toutes les questions, Composer t'a créé un fichier très important :

composer.json

Ce fichier contient toutes les informations relatives à ton projet que tu as renseignées dans ton terminal. Il contiendra également la liste des dépendances à gérer.

Voici le contenu de ce fichier :
````
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
````
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
````
composer require swiftmailer/swiftmailer
````
Ici pour installer la dépendance swifmailer (Pour envoyer des mails ;) ) 

# Symfony 

Symfony est un framework HTTP open source français, écrit en PHP objet. C'est le plus utilisé en France et l'un des plus utilisés dans le monde. Il en est à sa version 4.

# Démarrer un projet Symfony 

Symfony 4, tout en restant cohérent par rapport au 3, change légèrement de philosophie et de manière de faire. Afin de bien démarrer un projet Symfony, rien de mieux que la documentation officielle. Ces ressources t'expliquent comment les composants principaux de Symfony ont changé et surtout comment bien démarrer un nouveau projet Symfony.

https://symfony.com/doc/current/setup.html

## Installation de symfony dans le dossier projet 

Il est possible d'installer deux version de symfony, une version légère qui n'est constitué que du squelette de Symfony, pour faire de petites appli, des api, de petites fonctionnalités etc .. et une version lourde qui contient tout le necessaire pour développer une application web au complet. 

On va passer par composer pour l'installation de Symfony. 
Pour la version légere : 
```cli 
composer create-project symfony/skeleton my-project
```
Et pour la version lourde (recommandée pour nous au moins au début ) : 
```cli 
composer create-project symfony/website-skeleton my-project
```
La dernière partie de ces deux commandes correspond au nom du projet, on peut donc le remplacer par le nom souhaité. 


Historiquement, Symfony utilise le pattern MVC comme architecture. Il est possible avec Symfony 4 d'utiliser d'autres types d'architecture (CQRS, middlewares...) mais la plus populaire reste encore MVC. 

## Utiliser l'outil console de Symfony 

Le framework Symfony vient avec tout un tas de commandes qui vont te simplifier la vie. En utilisant ton terminal de commande comme tu le faisait jusqu’a présent pour jouer avec Git, PHP ou composer, tu vas pouvoir utiliser des directives propres à Symfony.

Par exemple, pour lancer le serveur durant la phase de développement, execute la commande ci-dessous au nvieau de ton dossier de projet :
```cli
bin/console server:run
```
Astuce : Un bon développeur est souvent fainéant. Pour gagner du temps, lorsque tu utilises la console Symfony, tu peux taper seulement les premières lettres des termes, par ex. bin/console s:r (au lieu de php bin/console server:run). Cela fonctionne avec toutes les commandes, tant qu'il n'y a pas d'ambiguïté dans les lettres utilisées.

Tu peux maintenant te rendre sur l'URL suivante : http://localhost:8000/.

L'avantage de la commande server:run est qu'elle changera le numéro du port toute seule : Si localhost:8000 est déjà utilisé par une autre application, alors le nouveau serveur écoutera localhost:8001 !

Remarque : au passage, tu peux spécifier une adresse IP au démarrage de ton serveur, afin de pouvoir utiliser ton serveur depuis un autre ordinateur ou un portable. Cela peut être pratique, notamment pour tester la responsivité ! Pour trouver ton adresse IP de réseau local, pense à exécuter la commande ifconfig. Exemple :
```cli
bin/console s:r 192.168.1.20
```
Sans que tu t'en rendes compte, Symfony a fait pas mal de choses. Dans le dossier blog, tu trouveras un ensemble de fichiers et de dossiers qui constituent l'architecture de ton projet Symfony. On y trouve tous les outils nécessaires pour construire une plateforme sur des bases saines et solides. Jette un œil dans les deux ressources ci-dessous pour connaître le rôle de ces dossiers.

Astuce : Pour avoir accès à toutes tes options console possibles, tu peux utiliser cette commande :
```cli
bin/console list
```
Ou concernant ton environnement de projet :
```cli
bin/console about
```
## Récupérer un projet Symfony 

Quand tu récupères un projet existant, c'est le plus souvent en utilisant ```git clone```. Le dossier récupéré contient déjà toute l'infrastructure de dossiers de Symfony. Mais attention, il va te manquer un certain nombre de fichiers et de dossiers, notamment ceux ignorés dans le .gitignore.

Par exemple, le dossier vendor/ (généré par Composer), est ignoré. C'est sûrement le plus gros dossier du projet, car il contient les bibliothèques utilisées par l'application.

Pour récupérer ce dossier, tu devras donc lancer la commande (attention, il faut que tu sois dans le dossier du projet Symfony) :
```cli
    composer install
 ```
 Par la suite, tu auras éventuellement à personnaliser le fichier .env qui est un fichier de configuration qui t'est propre.

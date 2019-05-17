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
 
 ## Creer un premier controller 
  
 Un contrôleur en Symfony n'est pas différent d'un contrôleur en MVC : c'est tout simplement une classe.
Elle doit étendre ```Symfony\Bundle\FrameworkBundle\Controller\AbstractController```. Cette classe va également contenir des méthodes, qui seront appelées via le mécanisme de routing.

Par convention, le nom de ces classes est suffixé par Controller, par exemple, ForumController. Tu en déduiras facilement le nom du fichier, ForumController.php.

#### Mais où doit-on les mettre, ces contrôleurs ?

Dans l'absolu, un contrôleur peut vivre n'importe où. Certaines librairies fournissent même leurs propres contrôleurs, se trouvant donc dans le dossier ```vendor/nom_de_librairie/```.

Cependant, Symfony est configuré par défaut pour accueillir ceux que tu vas créer pour ton projet, dans le dossier ```src/Controller/``` (et éventuellement dans des sous-dossiers).

Par convention, le nom de ces classes est suffixé par Controller, par exemple, ForumController. Tu en déduiras facilement le nom du fichier, ForumController.php.


#### Exemple 

```php 

<?php
// src/Controller/BlogController.php
namespace App\Controller;


use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;


class BlogController extends AbstractController
{
    public function index()
    {
        return new Response(
            '<html><body>Blog Index</body></html>'
        );
    }
}
```
### L'objet Reponse 

Eh bien c'est un objet qui représente une réponse HTTP complète. Dans une réponse HTTP, on peut trouver des headers, un code de réponse (2xx, 3xx, ...) et éventuellement un contenu (HTML, XML, ...). C'est pareil pour l'objet Response (et plus précisément Symfony\Component\HttpFoundation\Response, qui est l'objet Response du composant "HttpFondation" de Symfony).

Le travail d'un contrôleur de Symfony est de retourner un objet de la classe Response ! Tout le temps. Quoi qu'il arrive. C'est une notion fondamentale à assimiler si tu veux comprendre le fonctionnement du framework.

Ici notre réponse HTML est "écrite en dure", mais ça changera par la suite.

## Implémenter un route 

Une route, finalement, ce n'est que de la configuration. Tu peux généralement la configurer de deux manières, au format YAML ou annotations.

### En yaml
 
 ```php
 //config/routes.yaml

//the "app_blog_index" route name is not important yet
app_blog_index:
    path: /blog/
    controller: App\Controller\BlogController::index
```
On a donc pour la méthode index() du controller BlogController, une route et un path associé. Mais ça serait un peu fastidieux de devoir aller à chaque fois dans routes.yaml pour configurer une nouvelle route ? C'est ici qu'entrent en jeu les annotations !

### Les annotations
Les annotations sont des commentaires à mettre dans notre code mais qui vont avoir un impact sur l’exécution. Attention, on est d'accord que des commentaires en PHP, ça n'a aucun impact à la base.

Cependant, les annotations sont des commentaires un peu particuliers, avec un format bien spécifique pour être reconnus comme tels. Symfony est capable d'aller lire ces annotations (elles doivent être écrites à des endroits bien particuliers) et de "se configurer" en fonctiond'elles !

Remarque : Il est possible d'utiliser uniquement la configuration de routes via le yaml, mais aussi via XML ou directement en PHP, mais ce sont les annotations qui sont recommandées dans les bonnes pratiques actuelles de Symfony.
Exemple d'annotation : 

```php
[...]
use Symfony\Component\Routing\Annotation\Route;


class BlogController extends AbstractController
{
    /**
     * @Route("/blog", name="blog_index")
    */
    public function index()
[...]
```

## Appeler un template TWIG 

Voilà, on a une page, mais on aimerait bien appeler une vue Twig plutôt que de mettre tout le HTML à la main dans l'objet Response...

Eh bien comme par hasard, c'est prévu de base dans ton installation de Symfony !

Pour cela, mets à jour ta méthode méthode index() dans ton contrôleur:

```php
[...]
public function index()
{
    return $this->render('blog/index.html.twig', [
            'owner' => 'Thomas',
    ]);
}
[...]
```
Et créer ta vue templates/blog/index.html.twig associée :
```html
<h1>{{ owner }}'s blog index</h1>
```
C'est tout bon ? Est-ce que tu es en train de te dire "mais c'est bizarre, on m'a dit qu'un contrôleur devait retourner un objet Response, et pourtant j'ai bien l'impression que je retourne le render d'un Twig" ? Attention, dans un contrôleur $this->render() renvoie bien un objet Response, et si tu en doutes, fais un ctrl+clic gauche sur le nom de la méthode dans PhpStorm !

Ici {{ owner }} est une variable envoyée depuis le contrôleur à ta vue. Pour l'exemple sa valeur est égale à Thomas. Par conséquent si tu actualises ta page, ta vue à la route blog/ affiche "Thomas's blog index".

## Rooting avancé 

#### Définir des paramètres
```php
// src/Controller/BlogController.php
namespace App\Controller;


use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\Routing\Annotation\Route;


class BlogController extends AbstractController
{
    [...]
    /**
    * @Route("/blog/list/{page}", name="blog_list")
    */
    public function list($page)
    {
        return $this->render('blog/list.html.twig', ['page' => $page]);
    }
}
```
Comme tu le vois, il est très simple de passer un paramètre via le router. Il suffit d'ajouter ce dernier entre accolades, ici {page}, où tu le souhaites dans ta route. Si tu saisis la route /blog/2, le paramètre {page} de la route va prendre la valeur 2, puis transférer cette valeur dans le paramètre du même nom, ici $page, de la méthode list(). Il est bien entendu possible d'ajouter autant de paramètres que nécessaires, généralement séparés par des slashes, par exemple /blog/{page}/{limit} . Fais cependant attention à la lisibilité de tes routes !

Tu peux ensuite utiliser la variable $page dans ton code comme bon te semble. Dans l'exemple, la donnée est envoyée en paramètre à Twig, mais tu pourrais t'en servir pour tout autre chose.

Exemple templates/blog/list.html.twig :
```html
<h1>Page number {{ page }}.</h1>
```

#### Contraintes sur les routes
Le mot clé requirements
La route que tu viens de définir nécessite donc un paramètre {page}. Logiquement, un numéro de page doit être un entier. Cependant, si tu n'indiques rien, il est tout à fait possible de saisir autre chose en paramètre, par ex /blog/deux/, ce qui pourrait poser des problèmes. Il faut donc interdire cela.


Heureusement, le router de Symfony embarque la possibilité d'imposer des limitations aux paramètres.
```php
  /**
   * @Route("/blog/list/{page}", requirements={"page"="\d+"}, name="blog_list")
   */
   public function list($page)
   {
       // ...
   }
```
L'exemple ci-dessus reprend le précédent, en ajoutant l'option requirements, qui permet d'ajouter des prérequis aux différents paramètres. Ici, pour le paramètre page, le prérequis est \d+. Tu reconnaîtras une expression régulière qui correspond à un chiffre répété de 1 à n fois, autrement dit un entier. Les requirements sont toujours définis par une regex, qui est un moyen simple et puissant de décrire des motifs variables.

Remarque : il est aussi possible d'écrire les requirements de manière plus condensée : @Route("/blog/list/{page<\d+>}", name="blog_list"). Cette écriture inline nécessite l'utilisation des chevrons <> pour encadrer la regex.

Au delà des requirements sur le format des paramètres, il est également possible de définir d'autres contraintes à tes routes.

Le mot clé methods
Certaines routes ne devraient être accessibles qu'en GET, d'autres qu'en POST (et pareillement avec d'autres verbes HTTP). L'annotation @Route peut donc prendre en paramètres les types de méthode acceptés. Par exemple, si tu souhaites créer un nouvel article (via un formulaire en POST uniquement), tu feras :

```@Route("/blog/article/new", methods={"POST"}, name="article_new")```
Si tu souhaites afficher un article en fonction de son identifiant, cela se fera plutôt par un lien, donc en GET.

```@Route("/blog/article/{id}", methods={"GET"}, name="article_show")```
Et si tu souhaites supprimer un article, tu pourrais limiter la méthode à DELETE uniquement.

```@Route("/blog/article/{id}", methods={"DELETE"}, name="article_delete")```
Comme tu le remarques, dans ce cas les routes article_show et article_delete ont la même définition /blog/article/{id}, mais la méthode définie n'étant pas la même, il n'y a pas de conflits entre elles. Le router les considère bien comme deux routes différentes.

Les méthodes peuvent également se cumuler : methods={"GET","POST"}). Définir la méthode dans les routes est donc un très bon moyen de conserver des URLs simples à écrire, tout en restant très spécifiques. Les routes d'API se reposent beaucoup sur ces contraintes de méthodes HTTP.

##### Valeur par defaut
```php
/**
 * @Route("/blog/list/{page}",
 *     requirements={"page"="\d+"},
 *     defaults={"page"=1},
 *     name="blog_list"
 * )
 */
 ```
Là encore, une notation plus concise existe : ```@Route("/blog/list/{page<\d+>?1}", name="blog_list")```. C'est cette fois-ci le symbole ```?``` qui permet de définir la valeur par défaut.

Une autre manière de faire consiste à définir les paramètres par défaut directement au niveau de la méthode :
```php
  /**
   * @Route("/blog/list/{page}", requirements={"page"="\d+"}, name="blog_list")
   */
   public function list($page = 1)
   {
       // ...
   }
```
## Utiliser les routes
Dans les contrôleurs
L'utilisation la plus fréquente des routes au sein d'un contrôleur est la redirection (en GET), suite par exemple à la validation d'un formulaire. Pour cela, tu utiliseras la méthode du contrôleur $this->redirectToRoute(), qui prend deux paramètres : le nom de la route et un tableau optionnel de paramètres à passer à cette route. Cette méthode renvoie un objet Response. N'oublie pas le mot clé return devant, sinon cela ne fonctionnera pas !
```php
    public function new()
    {
        // traitement d'un formulaire par exemple

        // redirection vers la page 'blog_list', correspondant à l'url /blog/list/5
        return $this->redirectToRoute('blog_list', ['page' => 5]);
    }
```
Il faut bien comprendre que la redirection revient au même que de taper l'url dans le navigateur, et donc le code de la méthode list() correspondant à la route /blog/list/5 va bien s'exécuter et l'éventuelle vue associée s'affichera donc. Pour rappel, après une redirection, les données disponibles (ici celle de la page new), sont perdues.

Dans twig
Dans tes vues, tu es souvent amené à afficher des liens internes. Pour définir la cible de ces liens, tu pourrais très bien entrer la route "en dur", par exemple ```html <a href="/blog/list/5">Afficher la page 5</a>```. Cependant, si tu modifies la définition de ta route dans ton contrôleur, alors ton lien ne fonctionnera alors plus. Dommage ! Une manière plus robuste de faire cela est d'utiliser la fonction ```{{ path() }}``` de twig, qui sert à faire appel à une route prédéfinie. Là encore, il faut lui donner le nom de la route (d'où l'intérêt de bien définir un name à chacune de tes routes) et une liste de paramètres (attention à la syntaxe particulière qui contient parenthèses et accolades).

```html <a href="{{ path('blog_list', {'page':5}) }}">Afficher la page 5</a>```
Maintenant, si tu modifies la définition de ta route blog_list, de /blog/list/{page} à /blog/page/{page} par exemple, l'URL du lien sera mise à jour automatiquement de /blog/list/5 à /blog/page/5.

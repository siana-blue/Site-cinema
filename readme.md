# Premier site web - Template pour de futurs projets

Ceci est mon premier projet d'application de mes acquis HTML, CSS et Javascript.

L'objectif de ce projet est très personnel, il s'agit de ma montée en compétences. Je souhaite par ce projet me fixer des règles d'organisation du code, une méthode de gestions des fichiers et une base pour la gestion de mes futurs projets web.

Dans ce fichier readme.md, j'expose les principes structurants que je me fixe. Ils sont peut-être non pertinents, non optimaux, mais je les ferai évoluer au fur et à mesure de mes lectures et de mon apprentissage. Cependant, tant qu'ils sont écrits ici, c'est que ce projet les suit. Je ne m'interdis pas d'en changer (et je pense le faire plus d'une fois).

Ecrire ma façon de penser est une bonne aide, car en relisant ce que j'ai écris plusieurs semaines plus tard, je comprends que je ne comprenais rien (déjà pour cette v0 après un mois de développement), c'est intéressant de voir son évolution !

## Concept du projet

L'application première de ce site web est une application concrète : la vitrine du cinéma associatif de ma ville dans lequel je suis bénévole. Cependant, je ne joints pas les images et je dépersonnalise toutes les ressources et tous les textes du projet, car je veux ici ne garder que la structure que je pourrai réutiliser pour de futurs projets (aussi je ne veux pas joindre de ressources potentiellement protégées).

Il s'agit donc du premier défi de ce projet : faciliter la conversion de la coquille de code en site web spécifique, par une simple manipulation d'épuration du code avant commit sur github (grâce au script obfuscat disponible dans un autre repository).

Ensuite, le principe de fonctionnement de ce site web repose sur les principes suivants :

- Le site doit être accessible sur toutes les machines et fonctionner sans Javascript. Ainsi, les pages HTML brutes doivent contenir toutes les informations nécessaires (films, horaires, informations sur l'association etc...) sans nécessiter de mise à jour (des films à l'affiche par exemple) par Javascript.
  ==> Il s'agit de respecter le Web Standards Model et ses quatres layers (graceful degradation), oui j'ai lu le MDN Curriculum :grin:
- Javascript ajoute des fonctionnalités de confort comme des filtres pour les utilisateurs (ciné jeunesse, filtres par genre etc...). Javascript permet également d'utiliser les formulaires de contact.

Tout ceci n'est pas prêt en v0, ce sont des projets.

## Standards (opinions personnelles amenées à évoluer)

Je présente ici mes propres standards de code, amenés à évoluer. Cette évolution dans mon organisation au fur et à mesure que j'apprendrai le développement web se verra au travers des différents commits qui inclueront ce fichier readme.md.

### HTML / CSS

#### Responsive design

Pour être responsive, les unités à utiliser sont (généralement) les suivantes :

- rem pour les propriétés margin, padding, line-height, font-size et border-radius. La font-size html est exprimée en vw.
- %, vw ou vh pour les propriétés width et height (surtout pas rem). % quand c'est possible, mais pour avoir des formes carrées ou rondes et qu'il est nécessaire d'avoir hauteur = largeur, généralement vw.
- px OK pour les borders

Cas particulier de height : peut être en px ou en rem contrairement à width.

Ensuite, pour les media queries, je n'ai pas encore de recette miracle. Il y en a un peu par tout pour tweaker ça et là ce qui le nécessite.

#### Utilisation et organisation des classes

Les classes doivent, dans la mesure du possible, avoir un rôle sémantique. Si je peux me passer d'une classe, j'essaye au maximum d'utiliser les sélecteurs de balises.
Cela fonctionne si les classes sont bien utilisées de façon "monolithique". C'est-à-dire qu'une classe est de préférence utilisée pour deux raisons :

- pour un bloc indépendant
- pour une spécificité, un modificateur

Ainsi, si c'est bien documenté en commentaires, on peut utiliser des sélecteurs de type "div h2 + p", car même si ce n'est pas très parlant, c'est juste le code du bloc en question, et on n'a pas trop besoin de relire et de comprendre cela. Ce qu'il faut c'est comprendre la mise en page du bloc, et les règles de structure qu'il doit respecter (et c'est en commentaire ou en documentation).
C'est un peu comme une fonction en programmation, il s'agit d'une boîte noire bien documentée, et commentée pour faciliter sa maintenance en cas de besoin, quand même.
Cela a l'avantage, je trouve, d'épurer le code HTML, ce qui est très important à mes yeux.
Des classes purement cosmétiques me font mal aux yeux ^^ (comme bootstraps, je ne peux pas trouver cela élégant dans une page HTML).

** Modèle BEM **

Les classes sont pensées selon le modèle BEM. J'utilise actuellement une notation du type :

`conteneur(balise ou classe)-composant--modificateur`

Il peut y avoir des classes hors BEM, qui servent de contexte. Par exemple, une classe peut servir de contexte pour une page (dans body, main, voire section). Ensuite, le modèle BEM s'applique sur les conteneurs situés dans ce contexte.

Par exemple, une section de classe "alaffiche" dispose ensuite de conteneurs dont un conteneur "affiche" qui subit ensuite les variations "affiche-principal" puis "affiche-principal--jeunesse" etc...
Ici, "alaffiche" est un contexte qui sera utilisé pour créer le gros bloc monolithique du fichier CSS correspondant, et à l'intérieur, le modèle BEM s'applique. En HTML, "alaffiche" est affecté à une section, tandis que "affiche" correspond aux articles qui sont à l'intérieur.

** Classes de body **

`theme-x`, `layout-x`

Ces classes sont intégrées au body et servent à spécifier le thème et le layout de la page. Le fichier base contient la mise en page des éléments de base génériques à tout le site. Le fichier theme y apporte des modifications. Pour le layout, c'est le fichier main qui gère cela directement (gestion du layout en général).

Exemples d'utilisation de theme :

- Indiquer un background particulier pour la page
- Changer la couleur de la police de la page

Exemples d'utilisation de layout :

- Le bandeau aside de la page d'accueil peut être placé à un autre endroit sur les autres pages que l'accueil.

#### Arborescence de fichiers

Je me fonde sur les recommandations de la documentation SASS.

** Gestion du layout **

Le dossier layout contient les fichiers qui gèrent les composants génériques d'une page, ceux qu'on retrouve sur toutes les pages HTML de manière quasiment similaire (header, nav...). Les objets sont intégralement définis dans ces fichiers, sauf pour le cas du main.

Les fichiers du dossier components contiennent les modules que l'on peut retrouver sur les différentes pages, et généralement intégrés dans le main. Ils décrivent le style interne au bloc du composant (la boîte noire monolithique, qui recourt le moins possible aux classes). Par contre, ils ne gèrent pas leur positionnement dans la page.

Le positionnement des composants dans la page est géré dans le fichier \_main.scss. Ainsi, on doit retrouver la classe de chaque composant dans ce fichier, sous la déclaration de la grille flexible dans la classe main. C'est un choix pour aller dans le sens de modularité des composants. Si la propriété `grid-row` est définie dans le composant, ce n'est pas réutilisable dans une autre grille. Or, le composant doit être un objet indépendant.
C'est bien au main d'organiser les composants qui le composent, dans le contexte du site web spécifique.

Ainsi, les fichiers du dossier layout sont pensés pour être spécifiques au site web considéré, tandis que les fichiers du dossier components sont réutilisables, comme une bibliothèque externe.

#### Conventions de nommage

- Pour le BEM : `conteneur(balise ou classe)-composant--modificateur`
- Les classes en majuscule : ce sont des classes non-sémantiques (exceptions), qui ont une fonction de mise en forme particulière parmi une liste prédéfinie (d'un total d'1 item pour le moment) :

  - WRAPPED : cette classe sert à indiquer qu'un élément qui bénéficie d'une mise en forme particulière, ne doit pas se la voir appliquée ici car c'est son parent qui s'en charge (et qui généralement utilise la même mixin).

### Javascript

Toute motivée que j'étais, j'ai écrit un roman sur mes pratiques en HTML/CSS. Pour Javascript, c'est plus simple : il faut que ça marche !
Cette section me servira aussi de bloc-notes de bonnes pratiques à l'avenir, mais entre obfuscat et le burger menu, pour l'instant c'est assez simple.

## Notes d'amélioration

Il faudrait supprimer .dropdown pour utiliser .unfold > ul à la place et ainsi éviter d'utiliser deux classes.

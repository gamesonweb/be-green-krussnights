# Documentation

Documentation du jeu Greenights.

## Table des matières
- [Remerciements](#remerciements)
- [Synopsis](#synopsis)
- [Contrôles](#contrôles)
- [Performance](#performance)
- [Comment jouer](#comment-jouer)
- [Making of Greenights](#making-of-greenights)
  - [De l'idée au jeu](#de-lidée-au-jeu)
  - [Création du prototype](#création-du-prototype)
    - [Création mappe](#création-mappe)
    - [Caméra de jeu](#caméra-de-jeu)
    - [Ennemis et pathfinding](#ennemis-et-pathfinding)
    - [Unités du joueur](#unités-du-joueur)
    - [Interactions entre ennemis et joueur](#interactions-entre-ennemis-et-joueur)
    - [Création de la GUI de jeu](#création-de-la-gui-de-jeu)
    - [Sprites et animations](#sprites-et-animations)
    - [Son du jeu](#son-du-jeu)
      - [Effets sonores (SFX)](#effets-sonores-sfx)
      - [Musique](#musique)
      - [Voix des personnages](#voix-des-personnages)
    - [Chargement des assets](#chargement-des-assets)
    - [Mecaniques codées](#mecaniques-codées)
    - [Menu contextuel unité](#menu-contextuel-unité)
    - [Retours sur le prototype](#retours-sur-le-prototype)
  - [Evolutions suite au prototype](#evolutions-suite-au-prototype)
    - [Plus d'info sur le menu contextuel](#plus-dinfo-sur-le-menu-contextuel)
    - [Priorité attaques](#priorité-attaques)
    - [Textures](#textures)
    - [Detection des impactes](#detection-des-impactes)
    - [Menu de jeu](#menu-de-jeu)
    - [Sauvegarde des progrès de jeu](#sauvegarde-des-progrès-de-jeu)
    - [Optimisation](#optimisation)
  - [Liens vers les différentes versions du jeu](#liens-vers-les-différentes-versions-du-jeu)
- [Sources utilisées](#sources-utilisées)
- [Tutoriels et codes de reference](#tutoriels-et-codes-de-reference)
- [Auteur](#auteur)
  - [Moi et le concours](#moi-et-le-concours)


## Remerciements

Je tiens à commencer par des remerciements parce que même si j'ai absolument adoré developper ce jeu sur lequel j'ai passé un nombre incalculable de journées, j'en serais possibilement pas arrivé à là si ce n'était pas grâce aux personnes suivantes :
  
- Les sponsors CGI pour avoir mis en place ce concours sans lequels je n'aurais peut-être jamais découvert BabylonJS et dévéloppé le jeu.
- Les developpeurs BabylonJS pour avoir crée ce framework, je n'aurais jamais cru que je pouvais créer un jeu aussi complexe dans mon langage de programmation préféré, le javascript.
- M.Buffa pour nous avoir bien fait de la pub sur ce concours me permettant de le découvrir, mais aussi pour ses mots d'encouragement et retours constructifs qui m'ont permis d'avancer dans la bonne voie.
- Mes amis sur discord pour m'avoir motivé à commencer le dévéloppement du jeu et qui m'ont suivi tout le long du parcours.
- Mes collegues à l'université et amis proches qui ont testé et m'ont donné des retours m'aidant à leur façon.

Sans toutes ces personnes, j'en serais pas là, je vous remercie.

## Synopsis

Vous êtes le résident d'une petite ville située au milieu de la fôret de Gaul. Vous vivez une vie calme en harmonie avec la nature qui l'entoure. 
  
Un jour, ce calme a été interrompu par des bandits. Sans demander l'avis de personne, ils ont commencé à couper des arbres, chasser des animaux et troubler l'ecosystème de la fôret.  
  
Vous avez essayé de raisonner avec eux, mais sans succès... Si vous ne faites rien, la fôret sera perdue à plus jamais !
* Formez une équipe de combattants prêts à proteger l'environnement à vos côtés !
* Recrutez au fur à mesure de votre aventure jusqu'à 45 héros différents eparpillés dans le monde pour faire face à des ennemis de plus en plus puissants !
* Affrontez 90 types d'ennemis différents dans 97 niveaux qui testeront vos reflexes et votre sens stratégique !
* Fôret, grotte, desert et plus ! Utilisez les différents environnements à votre avantage, ne négligez pas le champ de bataille et utilisez le terrain pour repousser les forces ennemis dans ce Tower Defense dynamique !

## Contrôles
Le jeu se joue exclusivement à la souris.  
Il est possible de mettre le jeu en pause avec le bouton "Echappe".

## Performance
Veuillez "zoomer" la page de jeu lorsque vous remarquez une perte de FPS, cela diminuera la qualité d'image du jeu mais augmentera la performance.  
Si vous ne voyez pas le texte de jeu (ex : écran trop petit) vous pouvez choisir de "dezoomer" le jeu, ceci ameliore la qualité d'image au risque de diminuer la performance.

## Comment jouer

![TUTORIAL 1](https://i.imgur.com/z91E73L.jpeg)
![TUTORIAL 2](https://i.imgur.com/8Mbop18.jpeg)
![TUTORIAL 3](https://i.imgur.com/7wVSkqa.jpeg)
![TUTORIAL 4](https://i.imgur.com/hKzcdma.jpg)
![TUTORIAL 7](https://imgur.com/il5NZ1H.jpeg)
![TUTORIAL 6](https://imgur.com/fugHSe5.jpeg)
![TUTORIAL 5](https://i.imgur.com/oNB22Fj.jpeg)

## Making of Greenights
### De l'idée au jeu
Après avoir terminé le concours de Games on web 2022, je me suis trouvé très impressionné des capacités de BabylonJS ce qui m'a ramené dans mon temps libre suite au concours à jouer avec les limites de la bibliothèque.  
  
Un jour, pendant que je jouais à Arknights, un jeu mobile dont je suis très fan, je me suis demandé si je pouvais le récréer avec BabylonJS pour refaire ma propre version et créer mes propres niveaux. Après tout, ça ne paraissais pas si compliqué que ça, le jeu est en 2 dimensions et les personnages se placent sur une grille bien definie, donc niveau physique (une des choses les plus compliquées à mon avis) je n'allais pas avoir de difficulté. 
  
Avec le support de mes amis qui étaient curieux de voir un Arknights "du pauvre", je me suis donc lancé sur le developpement.

### Création du prototype
#### Création mappe
J'ai commencé par créer une grille correspondant à un niveau. Cette grille est definie (dans le code) par une matrice et chaque élément du tableau sera une boite dessinée sur la scène, il y a 4 types de boites :
- boites surelevées qui correspond aux zones où on peut placer les unités rangés.
- boites au sol qui correspond aux zones où on peut placer les unités au sol et où les ennemis peuvent bouger.
- boites au sol noires correspondant au sol où on peut rien placer mais les ennemis peuvent se déplacer.
- boites surelevées noires correspondant au décor où on peut rien placer.  

#### Caméra de jeu
Ensuite, j'ai placé une camera universelle juste dessus la grille, j'ai eu quelques problèmes pour trouver le bon angle de la camera pour avoir une vue un peu penchée permettant un effet de distance sur le champ de bataille. Si je me contentais d'avoir une vue d'en haut, on n'aurait pas pu faire la difference entre les boites surelevées et celles au sol.  
#### Ennemis et pathfinding
Une fois le champ de bataille crée, il était temps de créer les ennemis.  
J'ai placé des cubes sur la mappe et à travers une bibliothéque de pathfinding à laquelle on passe une matrice definissant les zones sur lequelles on peut bouger ou pas (j'ai donc pu utiliser ma matrice definie précedemment), je les ai bougé donc d'un point x à un point y en les faisant passer uniquement par les boites au sol.
Une fois cela fait, j'ai transformé les points x en spawn d'ennemis (indiqués par des boites rouges) et les points y en point de défense (indiqués par les boites bleues).

#### Unités du joueur
Le jeu est un tower defense dont le but est d'utiliser ses unités pour se defendre des ennemis, il est important donc de mettre un place un mécanisme qui permet de cliquer d'une liste un personnage et le drag & drop sur le terrain pour le placer.
J'ai repris un [playground de Babylon pour drag & drop les meshes](https://www.babylonjs-playground.com/#7CBW04), puis j'ai crée une liste de personnages avec la GUI Babylon. J'ai fait en sorte que quand on clique sur un personnage, une meshe est crée sur l'emplacement de la souris que l'on pourra deplacer. Quand on lachera la souris sur le champ de bataille, l'unité sera placée.  

#### Interactions entre ennemis et joueur
Nous avons maintenant des ennemis qui bougent vers des objectifs et des unités que l'on peut placer pour les arrêter. Il faut maintenant les faire interagir entre eux.  
J'ai crée un système de bloque qui permet aux unités alliées sur le chemin des ennemis de les bloquer (on vérifie si la position de la meshe de l'ennemi et de l'unité sont proches), une fois un ennemi bloqué, il essayera d'attaquer l'unité qui le bloque.  
En ce qui concerne les unités, elles auront une portée d'attaque qui definit sur quelles cases qui les entourent elles vont attaquer. Elles vont cependant attaquer les ennemis qu'elles bloquent en priorité.  
J'ai ensuite mis en place un système de dégâts permettant aux personnages de s'entre tuer. Si une unité meurt, tous les ennemis qu'elle bloque ne seront plus bloqués. Si un ennemi bloqué meurt, l'unité pourra bloquer quelqu'un d'autre.

#### Création de la GUI de jeu
Avec les bases de jeu crée, j'ai crée quelques éléments de GUI pour permettre aux joueurs de savoir ce qui se passe :
- J'ai tout d'abord ajouté des barres de vie à chaque personnage sur la mappe qui se mettent à jour avec les points de vie. J'ai eu pas mal de problèmes avec les barres de vie (placements différents par rapport à la position du personnage sur la mappe, chevauchements) que je n'ai pas completement resolu à ce jour mais qui ne génent pas le gameplay.
- Ensuite, j'ai crée des indicateurs de progression du niveau en haut affichant le nombre de vies du joueur restants (chaque fois qu'un ennemi traverse une boite bleue, le joueur perd une vie et s'il perd toutes ses vies c'est game over) et le nombre d'ennemis tués sur le total d'ennemis du niveau (si le joueur tue tous les ennemis alors il a gagné)
- J'ai mis en place des boutons qui permettent de mettre le jeu en pause et d'augmenter/diminuer la vitesse de jeu (donc les ennemis et les personnages attaqueront et bougeront à des intervalles correspondants à la vitesse selectionnée)

#### Sprites et animations
Pour une prémière version j'aurai pu me contenter de jouer avec des cubes, mais je ne voulais pas quelque chose de si moche. Arknights utilise des personnages 2D, donc j'ai cherché comment utiliser des sprites sur Babylon. Heureusement, il est très simple avec un [SpriteManager](https://doc.babylonjs.com/features/featuresDeepDive/sprites/sprite_manager) d'afficher des sprites sur une scene et les animer (en choisissant de quel sprite jusqu'à quel sprite l'animation s'execute). Il ne me restait donc que de trouver les sprites. Arknights possède une bibliothéque en ligne qui affiche tous les personnages et les animations associées sous fond vert. Après plusieurs recherches sur outils en lignes pouvant m'aider dans ma tâche et essais, j'ai mis en place un processus specifique pour convertir en spritesheets utilisables les personnages 2D:
1. J'ai enregistré les animations en GIF à 15 FPS (j'ai decidé d'utiliser 15 FPS parce que plus de FPS allait créer des spritesheet immenses qui étaient simplement impossibles à gérer niveau performance).
2. J'ai ensuite supprimé les fonds verts des GIF.
3. Enfin, j'ai converti les GIF en spritesheet tout en notant quelque part où chaque animation commençait et se terminait (exemple : animation d'attaque pour personnage x commence au sprite 0 et termine au sprite 15, puis celle de mort commence au sprite 16 etc.)

J'ai suivi ce processus pour tous les sprites que vous pourrez voir dans le jeu et le processus n'a pas changé depuis le prototype jusqu'à la dernière version.  
  
***Début du prototype, avec chiens utilisant les sprites***  
<img src="https://imgur.com/H1OMPxd.jpg" alt= “” width="70%">

#### Son du jeu
Pour une expérience de jeu optimale, j'ai aussi fait attention à mettre des effets sonores, de la musique et des voix pour les unités depuis le prototype.  
##### Effets sonores (SFX)
J'ai decidé de lancer des sfx pour plusieurs actions (ennemi ou allié) : quand une attaque est lancée, quand l'attaque connecte avec l'ennemi, quand un skill est activé. Ensuite, je joue des sfx assez forts quand un allié est mort et quand un ennemi atteint une boite bleue pour faire réagir le joueur qui n'était peut-être pas attentif sur la zone du champ de bataille impactée.
##### Musique
Pour la musique, je ne voulais absolument pas une musique qui termine et se relance depuis le début, j'ai donc fait l'effort de créer une musique qui fait un loop qu'on ne remarque pas donnant l'impression d'une musique infinie. Pour faire cela, je joue donc d'abord un bout de musique au début, et ensuite un autre bout de musique qui boucle sans qu'on remarque permettant une musique infinie.
##### Voix des personnages
Chaque personnage parlera pour certaines actions : quand il active un skill, quand la bataille commence (celui qui dira quelque chose pour ce cas, sera choisi aléatoirement), quand son propre menu contextuel est selectionné, quand il est placé sur la mappe.  
Les voix des personnages (en japonais, parce qu'on est très manga/anime par ici!) donnent beaucoup de caractères aux unités que l'on joue.

#### Chargement des assets
J'ai utilisé l'asset manager pour charger tous les asset de jeu, j'ai été obligé de faire cela car sinon le jeu allait freeze pendant un moment quand la mappe commence pour charger les asset les plus lourds comme les sprites.  
J'ai du être créatif pour pouvoir charger mes sprite manager avec l'asset manager car il n'y avait aucune doc sur le sujet sur les forum Babylon.  
Je suis donc allé voir le code source de SpriteManager et j'en ai donc deduit que je pouvais charger les spritesheet en tant que textures et ensuite les affecter manuellement aux sprite managers par la suite.

#### Mecaniques codées 
Voici une liste non exhaustive des mécaniques de jeu présentes sur cette version :
* Quand on veut placer un personnage, les boites sur lequelles on peut le poser seront coloriées en vert.
* Pour placer un personnage, il faut avoir assez de DP (monnaie de jeu), sinon on ne pourra pas effectuer l'action.
* Chaque ennemi et personnage peut attaquer avec deux types de dégâts possibles :
  * Physique : est diminué par la Defense (DEF) de l'unité attaquée.
  * Magique : est diminué par la Resistence (RES) de l'unité attaquée.
* Quand une unité alliée est tuée, elle sera remise sur la liste des personnages que l'on peut jouer mais ne sera pas disponible avant la fin d'un timer.
* Un ennemi qui possède une portée va attaquer la première unité du joueur sur sa portée. S'il y a plusieurs unités, elle attaquera celle qui a été positionnée sur le terrain en dernière.
* Le joueur peut positionner des personnages soigneurs qui peuvent regenerer les points de vie des autres unités mais ne peuvent pas attaquer les ennemis.
* Chaque unité alliée possède un skill (qui se charge avec une barre GUI jaune) qui leur confère des capacités spéciales (pour cette version, les skills sont simplistes donc juste des augmentation de stats).

#### Menu contextuel unité
Pour permettre aux joueurs d'interagir avec leurs unités sur le terrain, j'ai fait en sorte que quand on clique sur une unité, on peut effectuer des actions contextuelles, de plus on pourra aussi voir la porté de l'unité en temps réel.  
Avec ce menu on peut retirer une unité dont on a plus besoin du terrain ou activer son skill s'il est prêt.  

***Menu contextuel d'une unité***  
<img src="https://i.imgur.com/KzEzZT8.png" alt= “” width="40%">

#### Retours sur le prototype
J'ai eu des retours plutôt positifs sur le prototype, cependant j'ai eu aussi des problèmes qui ont été remontés :
* La GUI s'étire si on est dans une résolution non conventionnelle
  * Solution : j'ai forcé le jeu a garder une résolution optimale de 16:9 sur n'importe quel écran.
* Le système de bloque est très bogué (plus d'ennemis bloqués que le nb de BLOCK, ennemis touchés la où il ne faudrait pas)

### Evolutions suite au prototype
Apart rendre le jeu plus dense mecaniquement, j'ai aussi modifié des choses pour améliorer les graphismes du jeu et le rendre plus agréable à jouer.
#### Plus d'info sur le menu contextuel
J'ai ajouté ensuite des informations du personnage sur le menu contextuel permettant au joueur de connaitre les stats du personnage ainsi ce que son skill fait.
#### Priorité attaques
J'ai decidé de faire en sorte que les unités attaquent les ennemis les plus proches de leur dernière destination cela permettant de se concentrer sur les ennemis qui sont les plus proches des boites bleue dans des situations dangereuses.
#### Textures
Pour rendre le jeu plus joli, j'ai ajouté des textures au champ de bataille, les textures changent par rapport au type de champ de bataille sur lequelle on se trouve, j'ai aussi ajouté des skybox appropriées
#### Detection des impactes
Pour permettre aux joueurs de mieux comprendre ce qui a été touché par des attaques, j'ai fait que quand un personnage se fait toucher, son sprite devient rouge pendant un instant. De plus, la barre vie ne diminue pas instantanéement mais progressivement pour faire remarquer les dégats aux joueurs. Enfin, j'ai ajouté des projectiles aux attaques de portée pour pouvoir suivre ce genre d'attaques des yeux.
#### Menu de jeu
Pour naviguer entre les niveaux, j'ai crée une autre scene qui fait de menu principal, ce menu permet de choisir les niveaux et former son équipe. De plus, si un joueur termine un niveau, j'ai fait en sorte qu'on puisse ouvrir le menu principal sur le chapitre du niveau en question diminuant le nombre de clicks necessaires si la scene se chargeait toujours à la page principale.
#### Sauvegarde des progrès de jeu
Le jeu sauvegarde les progrès du joueur dans le localStorage car le jeu est très long et je ne voulais pas faire perdre les progrès aux joueurs. De plus, j'ai fait en sorte qu'on puisse exporter un json correspondant aux progrès de jeu si le joueur souhaite changer de navigateur ou d'ordinateur et garder ses progrès.
#### Optimisation
Pour optimiser le jeu, j'ai du changer la taille de mes spritesheets plusieurs fois car elles avaient des tailles abérrantes. J'ai diminué leur taille le plus possible, je les ai compressées et ensuite je les ai converties en webp qui est un format très leger permettant la transparence.

### Liens vers les différentes versions du jeu
Il faut noter que c'est des liens vers des repo, si vous souhaitez essayer les différentes version, il faudra télécharger le code et lancer index.html :
- 1 seul niveau, pas d'écran d'accueil : [prototype](https://github.com/KrussX/Krussnights/tree/76f2d1370435bbef9be98f46b89feeb42d5c0b39).
- plusieurs niveaux, écran d'accueil minimaliste : [alpha](https://github.com/KrussX/Krussnights/tree/c9cbc812858858ef8f0c79b7880b5471227d31e8)
- amélioration graphismes, textures, gui et plus de profondeur dans le gameplay : [beta](https://github.com/KrussX/Krussnights/tree/a752b7d0925593d45abcdc547d38b17b62eb299d).
- version du jeu classique : [Krussnights](https://github.com/KrussX/Krussnights).
- version pour le concours : [Greenights](https://github.com/saad-ahmed98/Greenights).

## Sources utilisées
- SFX et voix : [aceship](https://aceship.github.io/AN-EN-Tags/index.html).
- Backgrounds : [aceship](https://aceship.github.io/AN-EN-Tags/akgallery.html).
- Sprites enregistrées sous forme de GIFs à partir de : [arknights SD](https://flashmercurymcfly.github.io/Arknights-SD-Viewer/).
- GIFs converties en spritesheets avec : [gif2sprite](https://jacklehamster.github.io/utils/gif2sprite/).
- Bibliothèque pour le pathfinding : [PathFinding.js](https://github.com/qiao/PathFinding.js/).
- icones GUI : [game-icons](https://game-icons.net/).

## Tutoriels et codes de reference
- Drag & drop (pour placer les unités) : [babylonjs drag demo](https://www.babylonjs-playground.com/#7CBW04)
- Code écran de chargement : [SomeBabylonGame](https://github.com/saad-ahmed98/SomeBabylonGame)
- Dessiner ligne progressivement (pour montrer la route des ennemis quand ils apparaissent) : [babylonjs playground](https://playground.babylonjs.com/#5Q3FLL)
- Changer le rgb d'un sprite (pour montrer quand un sprite s'est fait toucher) : [Can I set sprite transparency](https://forum.babylonjs.com/t/can-i-set-sprite-transparency/30748)
- Preload GUI image (pour charger les images GUI avant le début d'une bataille) : [How to preload a GUI image?](https://forum.babylonjs.com/t/how-to-preload-a-gui-image/9858)
- Charger une sprite et l'utiliser dans le jeu : [SpriteManager](https://doc.babylonjs.com/features/featuresDeepDive/sprites/sprite_manager)
- Optimisation d'une scene : [optimize your scene](https://doc.babylonjs.com/features/featuresDeepDive/scene/optimize_your_scene)
- Jouer du son : [Audio](https://doc.babylonjs.com/features/featuresDeepDive/audio/playingSoundsMusic)

## Auteur
Saad El Din Ahmed

### Moi et le concours
Actuellement, alternant en M2 MIAGE à l'université de Nice Côté d'Azur, je suis un étudiant très discret qui passe parfois inaperçu au sein de sa promo, mais qui ne manque pas de motivation quand il faut travailler.  
  
J'ai toujours été un avide gamer, j'ai eu ma première console (Nintendo SNES) à 6 ans et depuis, je n'ai jamais arrêté de jouer et à me divertir avec les jeux-vidéos.  
Comme tout gamer, il y a eu un ou plusieurs moments où on a imaginé des scénarios ou des mécaniques de fou pour un hypothétique jeu sans pourtant avoir les capacités pour créer ce jeu, et rejoindre une formation informatique a renforcé l'idée, car quand on se documente un minimum sur le processus de création d'un jeu, on remarque que c'est très difficile et compliqué du côté technique, mais aussi côté design.  
  
Un jour par contre tout a changé, en M1 MIAGE, j'ai eu l'opportunité d'avoir des cours optionnels en développement 3D. Ces cours, enseignés par M.Buffa, nous ont appris les principes du développement 3D tout en utilisant un langage qui était familier à tout informaticien : le javascript.  
Cette année-là, avec Yessine Ben El Bey et Wajdi Gaiech, nous avons utilisé ce que l'on a appris avec les cours de 3D et présenté notre jeu SomeBabylonGame au concours.  
C'était une expérience très enrichissante, et même si nous n'avions aucune confiance en notre jeu (je me rappelle que pendant la remise de prix, on était soulagés quand la dernière team a été annoncée parce qu'on pensait vraiment qu'on pouvait potentiellement être les derniers!) nous avons été surpris en découvrant que nous avions atteint le podium en gagnant le prix "Coup de coeur CGI".  
Cela m'a montré que je sous-estimais mon potentiel, mais aussi m'a donné la motivation d'essayer de faire plus, peut-être créer l'un des jeux que je me disais incapable de faire à l'époque...  
  
J'ai donc passé du temps après le concours à jouer un peu avec l'engine BabylonJS et je me suis enfin décidé à créer un jeu complet.  
En M2, notre planning est très chargé, de plus, nous avons aussi des alternances qui occupent la majorité de notre temps. Je n'ai donc pas réussi à former une équipe cette fois-ci, et même pour moi, il allait peut-être se révéler compliqué de consacrer mon temps libre au jeu surtout si j'allais tout faire solo. Je me suis donc pris tôt et j'ai commencé à développer aussi tôt que septembre. J'avais commencé pour le fun, mais plus je travaillais sur le jeu, plus de choses je voulais ajouter pour rendre l'expérience utilisateur meilleure et je me suis retrouvé à consacrer tout mon temps libre à coder.  
  
Finalement, je suis très heureux de ce que j'ai fait et j'ai eu des retours plutôt positifs. J'espère que vous allez aussi aimer mon jeu et vous divertir !


# Documentation

Documentation du jeu Greenights.

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
Il est possible de mettre le jeu en pause avec le bouton "Echappe"

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
- boites surelevées qui correspond aux zones où on peut placer les unités rangé.
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
Pour une prémière version j'aurai pu me contenter de jouer avec des cubes, mais je ne voulais pas quelque chose de si moche. Arknights utilise des personnages 2D, donc j'ai cherché comment utiliser des sprites sur Babylon. Heureusement, il est très simple avec un [SpriteManager](https://doc.babylonjs.com/features/featuresDeepDive/sprites/sprite_manager) d'afficher des sprites sur une scene et les animer (en choisissant de quel sprite jusqu'à quel sprite l'animation s'execute). Il ne me restait donc que de trouver les sprites. Arknights possède une bibliothéque en ligne qui affiche tous les personnages et les animations associées sous fond vert, j'ai donc suivi un processus specifique pour les convertir en spritesheets utilisables :
1. J'ai enregistré les animations en GIF à 15 FPS (j'ai decidé d'utiliser 15 FPS parce que plus allait créer des spritesheet immenses qui étaient simplement impossibles à gérer niveau performance)
2. J'ai ensuite, supprimé les fonds verts des GIF.
3. Enfin, j'ai converti les GIF en spritesheet tout en notant quelque part où chaque animation commençait et se terminait (exemple : animation d'attaque pour personnage x commence au sprite 0 et termine au sprite 15, puis celle de mort commence au sprite 16 etc.)

J'ai suivi ce processus pour tous les sprites que vous pourrez voir dans le jeu et le processus n'a pas changé depuis le prototype jusqu'à la dernière version.  
  
***Début du prototype, avec chiens utilisant les sprites***  
<img src="https://imgur.com/H1OMPxd.jpg" alt= “” width="70%">

#### Son du jeu
Pour une expérience de jeu optimale, j'ai aussi fait attention à mettre des effets sonores, de la musique et des voix pour les unités depuis le prototype.  
##### Effets sonores (SFX)
J'ai decidé de lancer des sfx pour plusieurs actions (ennemi ou allié) : quand une attaque est lancée, quand l'attaque connecte avec l'ennemi, quand un skill est activé. Ensuite, je joue des sfx assez forts quand un allié est mort et quand un ennemi atteint une boite bleue pour faire réagir le joueur qui n'était peut-être pas attentif sur la zone du champ de bataille impactée.
##### Musique
Pour la musique, je ne voulais absolument pas une musique qui termine et se relance depuis le début, j'ai donc fait l'effort de créer une musique qui fait un loop qu'on ne remarque pas. Pour faire cela, je joue donc d'abord un bout de musique au début, et ensuite un autre bout de musique qui se loop permettant une musique infinie.
##### Voix des personnages
Chaque personnage parlera pour certaines actions : quand il active un skill, quand la bataille commence (celui qui dira quelque chose pour ce cas, sera choisi aléatoirement), quand son propre menu contextuel est selectionné, quand il est placé sur la mappe.

#### Chargement des assets
J'ai utilisé l'asset manager pour charger tous les asset de jeu, j'ai été obligé de faire cela car sinon le jeu allait freeze pendant un moment quand la mappe commence pour charger les sprites.  
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


## Auteur
Saad el din Ahmed (Kruss)

1. Résumé de l'article

TODO (la c'est pas super bien organisé)

# Introduction

L'article *Flocks, Herds, and Schools: A Distributed Behavioral Model* de *Craig W. Reynolds* décrit comment simuler des flocks en considérant chaque individu à part sans avoir à scripter le mouvement de chaque individu, mais plutôt en fonction de l'environnement perçu par chaque individu. Le flock sera ainsi le résultat du mouvement de chaque individu. À première vue, la simulation d'un flock peut paraître difficile, car il y a beaucoup d'individu à simuler et très dur à maintenir (éviter les collisions entre toutes les particules à chaque frame par exemple). Ce papier propose une méthode pour simuler le comportement d'un individu. Le flock est le résultat du mouvement de cet individu appliqué à toutes les particules.

On considère ici le flock comme un ensemble de particules améliorées (boid). Chaque particule représente un individu (ici un oiseau) et possède des variables (couleur, localisation, velocité, etc) auxquelles on donne une forme (qui est plus significative visuellement et ajoute la notion d'orientation). Ici le vol est discret, on réalise des petits changements à chaque frame. De plus les individus possèdent une vitesse maximale. De plus, toutes les forces physiques ne sont pas modélisées (la gravité l'est par contre).

# Les bases de l'algorithme

Un oiseau souhaite suivre les autres pour se protéger des prédateurs, chercher de la nourriture, etc. Si un oiseau veut participer à un flock, il doit pouvoir coordoner ses mouvements avec les autres oiseaux.  De plus, on ne sait pas si un flock peut-être limité en nombre: il n'y a pas l'air d'avoir de limite. Un oiseau doit avoir conscience de lui-même, quelques voisins et de l'ensemble du flock. Ici, on considère le comportement de chaque oiseau dirigé par 3 forces :

+ Collision Avoidance, pour éviter de rentrer en collision avec les autres oiseaux.
+ Velocity (heading + speed => vecteur direction) Matching, pour rester à la vitesse des oiseaux voisins
+ Flock Centering, pour rester proche des oiseaux voisins et aller vers le centre du flock car y a la meme densité partout autour de lui.

On observe 2 forces opposées: l'une qui pousse l'oiseau àrester dans le flock, l'autre qui le pousse à éviter les autres oiseaux; il faut y prêter attention car cela peut amener l'oiseau à ne pas savoir où se déplacer.
Les trois forces agissent indépendament et propose une solution différente au choix de la direction à prendre. On peut alors décider d'atténuer plus ou moins telle ou telle force et les combiner pour obtenir le mouvement résultant des 3 forces, etc. Si moyenner ces trois forces est la solution la plus simple, Il semble plus intéressant de les prioritiser puis de les accumuler dans l'ordre jusqu'à atteindre une magnitude totale maximale, pour éviter les problèmes d'opposition évoqués précédemment. Par exemple, si on regarde en priorité la force de Collision Avoidance et qu'elle est de magnitude plus forte que celle maximale alors on suivra directement cette force pour éviter en priorité la collision et les autres forces seront temporairement non "écoutées".

# Un flock plus intelligent

On peut enfin ajouter des fonctionnalités comme l'initialisation de la position des obstacles, des oiseaux ou encore leur donner un but de migration au fur et à mesure. La plus intéressante et l'évitement d'objets. Il y a deux solutions, la première (force field concept) consiste à avoir une force de répulsion autour de l'objet à éviter, mais si l'oiseau arrive pile en face d'une ligne du champ, il ne sera pas repoussé mais ralenti. Ou si l'oiseau vole à coté d'un mur il n'a pas besoin de tourner même s'il est dans le champ. La seconde (steer-to-avoid), où l'oiseau ne considère que les obstables face à lui. S'il en trouve un, il calcule alors un vecteur afin d'éviter l'objet.

# Conclusion

Le système de simulation de flocking peut trouver des applications dans la simulation de traffic ou l'observation de flocks, troupeaux, etc. De plus il est possible d'améliorer l'algorithme en le parrallélisant ou en étant sur que N soit petit (en considérant seulement une partie du flock par exemple)

# MDS-DP-2324

## présentation DP => définition + formalisme (objectif : tous le même formalisme pour la suite du document) : Hugo

## présentation du Gang of Four (GoF) => historique : Maxence

## Singleton : Yannick (1/23)

> Singleton garantit qu’une classe n’a qu’une seule instance et fournit un point d’accès global à cette instance.

### Description du problème

Certaines applications possèdent des classes qui doivent être instanciées une seule et unique fois. C’est par exemple le cas d’une classe qui implémenterait un pilote pour un périphérique, ou encore un système de journalisation. En effet, instancier deux fois une classe servant de pilote à une imprimante provoquerait une surcharge inutile du système et des comportements incohérents.

On peut alors se demander comment créer une classe, utilisée plusieurs fois au sein de la même application , qui ne pourra être instancié qu’une seule fois ?

Une première solution, régulièrement utilisée, est d’instancier la classe dès le lancement de l’application dans une variable globale (c’est à dire une variable accessible depuis n’importe quel emplacement du programme). Cependant cette solution doit être évitée car en plus d’enfreindre le principe d’encapsulation elle comporte de nombreux inconvénients. En effet, rien ne garantit qu’un développeur n’instanciera pas une deuxième fois la classe à la place d’utiliser la variable globale définie. De plus, on est obligé d’instancier les variables globales dès le lancement de l’application et non à la demande (ce qui peut avoir un impact non négligeable sur la performance de l’application). Enfin, lorsqu’on arrive à plusieurs centaines de variables globales le développement devient rapidement ingérable surtout si plusieurs programmeurs travails simultanément.

Mais si on n’utilise pas ce stratagème comment faire ? C’est simple il suffit d’utiliser le design pattern Singleton.

### Définition de la solution

L’objectif est d’ajouter un contrôle sur le nombre d’instances que peut retourner une classe.

La première étape consiste à empêcher les développeurs d’utiliser le ou les constructeur(s) de la classe pour l’instancier. Pour cela il suffit de déclarer privé tous les constructeurs de la classe. Attention dans certains langages une classe sans constructeur possède un constructeur implicite par défaut (c’est notamment le cas de Java). Il faut donc que celui-ci soit déclaré explicitement en privé.

Une fois cette étape accomplie, il est possible d’instancier cette classe uniquement depuis elle même, ce qui n’a pas beaucoup de sens. Comment allons nous faire pour permettre aux développeurs de l’utiliser ?

Nous allons construire un pseudo constructeur. Pour cela il faut déclarer une méthode statique qui retournera un objet correspondant au type de la classe. L’avantage de cette méthode par rapport à un constructeur, est que l’on peut contrôler la valeur que l’on va retourner. Le fait que cette méthode soit déclarée statique permet de l’appeler sans posséder d’instance de cette classe. A noter que, par convention, ce pseudo constructeur est nommé getInstance.

Pour en finir avec le concept de base du Singleton voyons comment implémenter cette méthode.

Tout d’abord il faut créer un attribut statique qui va permettre de stocker l’unique instance de la classe. Ensuite, dans le pseudo constructeur on va tester cet attribut. Si celui-ci est nul alors on crée une instance de la classe et on stocke sa valeur dans cet attribut. Sinon c’est que l’attribut possède déjà une instance de la classe. Dans tous les cas la méthode retourne la valeur de l’attribut possédant l’unique instance de la classe.

### Conséquences

Jusqu’à maintenant nous avons passé sous silence un problème qui peut mettre en péril l’implémentation du pattern Singleton : le multithread (capacité pour un programme de lancer plusieurs traitements simultanés c'est à dire processus). En effet, si l’on implémente de façon basique le pattern Singleton, dans le cas d’un programme multithread, on peut se retrouver avec une classe Singleton possédant plusieurs instances.

Le problème réside dans l’enchaînement des instructions. Un premier processus exécute la fonction getInstance constate que l’attribut uniqueInstance est nul. Un deuxième processus s’exécute et lui aussi constate (via getInstance) que l’attribut uniqueInstance est nul. Il va donc créer une instance et retourner celle-ci. Lorsque le premier processus va reprendre son exécution il va à son tour créer une nouvelle instance (étant donné qu’il a déjà effectué le test sur l’attribut) et retourner celle-ci. On se retrouve alors avec deux instances pour une classe Singleton.

Pour résoudre ce problème, on dispose de deux solutions. La plus simple consiste à instancier l’attribut uniqueInstance dès sa déclaration dans la classe. Ainsi l’implémentation de la méthode getInstance se limite à retourner l’attribut uniqueInstance. Cette approche est fiable et simple à mettre en place mais on perd l’instanciation à la demande.

L’autre approche consiste à s’assurer que la fonction getInstance ne pourra être exécutée que par un seul processus à la fois. Chaque langage dispose de sa spécification pour indiquer la particularité de cette méthode. Par exemple, en Java on utilisera le mot clef « synchornized ». Cette solution bien que satisfaisante, peut réduire les performances d’un programme multithread. Il existe alors des stratagèmes suivant les langages pour pallier à cette lacune.

On peut alors se demander quand doit-on mettre en place ces solutions spécifiques aux multithread. Le meilleur conseil est de toujours implémenter le pattern Singleton comme si le programme utilisait le multithread. Car même si ce n’est pas le cas aujourd’hui, il se peut que dans le futur le multithread apparaisse dans votre application.

## Fabrique : Evhan (2/23)

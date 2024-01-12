# MDS-DP-2324

## présentation DP => définition + formalisme (objectif : tous le même formalisme pour la suite du document) : Hugo

Un patron de conception (ou Design Pattern en anglais) décrit une solution standard face à un problème récurrent dans la conception d’applications orientées objet. Elle sert de vocabulaire commun entre le concepteur et le programmeur pour parler de l’architecture logicielle d’un système informatique. 
Cela permet de répondre aux problèmes de conception grâce à des solutions déjà validées par des experts, ce qui fait gagner en temps et en qualité de conception ce qui diminue les coûts. Cela permet également de mettre en avant les bonnes pratiques de conception.

Chaque patron de conception suit un formalisme fixe : 
-	Nom : permet de l’identifier clairement.
-	Problématique : description du problème auquel il répond.
-	Solution : description de la solution souvent accompagnée d’un schéma UML.
-	Conséquences : les avantages et les inconvénients de cette solution.

Il aide à comprendre l’utilisation et la logique de chaque patron. Un aspect important est également l’orthogonalité, chaque patron correspond à une solution différente, qui ne répète pas les idées ou stratégies présentes dans d’autres patrons de conception. 

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

## Fabrique abstraite : Maxence (3/23)

## Adaptateur : Hugo (4/23)

## Pont : (5/23)

## Monteur : (6/23)

## Chaine de responsabilité : Hugo (7/23)

## Commande : Evhan (8/23)

## Composite : (9/23)

## Décorateur : Yannick (10/23)

> Le pattern Décorateur (Decorator) attache dynamiquement des responsabilités supplémentaires à un objet. Il fournit une alternative souple à l’héritage, pour étendre des fonctionnalités.

### Description du problème

Dans la programmation orientée objet, la façon la plus classique d’ajouter des fonctionnalités à une classe est d’utiliser l’héritage. Pourtant il arrive parfois de vouloir ajouter des fonctionnalités à une classe sans utiliser l’héritage. En effet, si l’on hérite d’une classe la redéfinition d’une méthode peut entraîner l’ajout de nouveaux bugs. On peut aussi être reticent à l’idée que des méthodes de la classe mère soient appelées directement depuis notre nouvelle classe.

De plus, l’héritage doit être utilisé avec parcimonie. Car si on abuse de ce principe de la programmation orientée objet, on aboutit rapidement à un modèle complexe contenant un grand nombre de classes.

Un autre souci de l’héritage est l’ajout de fonctionnalités de façon statique. En effet, l’héritage de classe se définit lors de l’écriture du programme et ne peut être modifié après la compilation. Or, dans certains cas, on peut vouloir rajouter des fonctionnalités de façon dynamique.

D’une manière générale on constate que l’ajout de fonctionnalités dans un programme s’avère parfois délicat et complexe. Ce problème peut être résolu si le développeur a identifié, dès la conception, qu’une partie de l’application serait sujette à de fortes évolutions. Il peut alors faciliter ces modifications en utilisant le pattern Décorateur. La puissance de ce pattern qui permet d’ajouter (ou modifier) des fonctionnalités facilement provient de la combinaison de l’héritage et de la composition. Ainsi les problèmes cités ci-dessus ne se posent plus lors de l’utilisation de ce pattern.

### Définition de la solution

La classe abstraite Composant définit le point de départ de ce diagramme. Plusieurs ComposantConcret peuvent hériter de Composant. Si l’on souhaite étendre (ou modifier) l’ensemble des fonctionnalités des ComposantConcret on peut créer un décorateur. Il s’agit d’une classe abstraite héritant de Composant et ayant un attribut de type Composant. De plus, Decorateur déclare abstraite la méthode dont l’on souhaite étendre les fonctionnalités. Pour ajouter des fonctionnalités à un ensemble de ComposantConcret on va créer des classes DecorateurConcret qui héritent de Decorateur. Un DecorateurConcret contient un constructeur permettant d’initialiser l’attribut composant présent dans le décorateur. Il faut ensuite que la classe DecorateurConcret redéfinisse la méthode déclarée abstraite dans le décorateur. Lors de cette redéfinition il est possible d’étendre les fonctionnalités en appelant la méthode de l’attribut composant et en ajoutant des traitements.

Suivant les besoins spécifiques de chacun ce pattern peut être adapté. En effet, il est tout à fait possible d’utiliser des interfaces pour le composant et le décorateur. Dans ce cas, les attributs et les méthodes seront définis dans les sous classes.

Bien sûr ce pattern utilise largement l’héritage mais il utilise aussi la composition grâce à l’attribut Composant présent dans le décorateur. C’est l’alliance de ces deux procédés qui permet à ce pattern d’être si efficace.

### Explication détaillée de la solution

Voyons plus concrètement comment fonctionne ce pattern en prenant un exemple de l’utilité de ce pattern. Tout d’abord on a une classe ComposantConcret qui possède une méthode chargée d’une fonctionnalité. Suivant l’objet créé on souhaite ajouter des traitements lors de l’appel de cette méthode. Cependant on ne doit pas modifier directement le corps de la méthode car certains objets utiliseront toujours l’ancienne version de cette méthode. Pour cela on peut créer un objet DécorateurConcret en passant à son constructeur notre objet ComposantConcret (dont l’on souhaite étendre les fonctionnalités). On peut ensuite redéfinir la méthode concernée et ajouter des traitements. On appelle la méthode du ComposantConcret puis on rajoute des fonctionnalités.

On obtient un objet ComposantConcret qui est emballé dans un DecorateurConcret. Ainsi si on appelle la méthode sur l’objet décorateur, celle-ci va appeler la méthode du composant concret, ajouter ses propres traitements et retourner le résultat. A noter, que si on appelle directement la méthode de l’objet ComposantConcret (sans passer par le décorateur) on utilise alors l’ancienne version de la méthode.

### Conséquences

Comme tous les patrons de conception, Décorateur ne doit pas être utilisé à tord et à travers. Mais lors de la conception de classes qui risquent d’évoluer fortement (ajout ou modification de fonctionnalités) celui-ci sera très utile. Il est donc important de bien réfléchir aux points sensibles de l’application qui risquent d’évoluer au fil du temps et cela dès la phase d’analyse.

Attention tout de même à l’utilisation des types concrets. Si votre application se base sur les types concrets d’objets utilisés dans le pattern décorateur cela posera des problèmes. En effet, une fois décoré un ComposantConcret aura pour type concret celui de son décorateur le plus externe.

De plus, lors de l’utilisation du pattern Décorateur, on constate qu’il est fastidieux de gérer tous les objets créés et de les décorer. C’est pour cette raison que ce pattern est souvent utilisé avec le pattern Fabrique ou Monteur qui répondent à cette problématique.

## Façade : (11/23)

## Flyweight : (12/23)

## Interpreter : (13/23)

## Iterator : (14/23)

## Mediator : (15/23)

## Memento : (16/23)

## Observer : Hugo (17/23)

## Prototype : (18/23)

## Proxy : (19/23)

## State : (20/23)

## Strategy : (21/23)

## Template method : (22/23)

## Visitor : (23/23)

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

### Problématique

Certaines applications possèdent des classes qui doivent être instanciées une seule et unique fois. C’est par exemple le cas d’une classe qui implémenterait un pilote pour un périphérique, ou encore un système de journalisation. En effet, instancier deux fois une classe servant de pilote à une imprimante provoquerait une surcharge inutile du système et des comportements incohérents.

On peut alors se demander comment créer une classe, utilisée plusieurs fois au sein de la même application , qui ne pourra être instancié qu’une seule fois ?

Une première solution, régulièrement utilisée, est d’instancier la classe dès le lancement de l’application dans une variable globale (c’est à dire une variable accessible depuis n’importe quel emplacement du programme). Cependant cette solution doit être évitée car en plus d’enfreindre le principe d’encapsulation elle comporte de nombreux inconvénients. En effet, rien ne garantit qu’un développeur n’instanciera pas une deuxième fois la classe à la place d’utiliser la variable globale définie. De plus, on est obligé d’instancier les variables globales dès le lancement de l’application et non à la demande (ce qui peut avoir un impact non négligeable sur la performance de l’application). Enfin, lorsqu’on arrive à plusieurs centaines de variables globales le développement devient rapidement ingérable surtout si plusieurs programmeurs travails simultanément.

Mais si on n’utilise pas ce stratagème comment faire ? C’est simple il suffit d’utiliser le design pattern Singleton.

### Solution

L’objectif est d’ajouter un contrôle sur le nombre d’instances que peut retourner une classe.

La première étape consiste à empêcher les développeurs d’utiliser le ou les constructeur(s) de la classe pour l’instancier. Pour cela il suffit de déclarer privé tous les constructeurs de la classe. Attention dans certains langages une classe sans constructeur possède un constructeur implicite par défaut (c’est notamment le cas de Java). Il faut donc que celui-ci soit déclaré explicitement en privé.

Une fois cette étape accomplie, il est possible d’instancier cette classe uniquement depuis elle même, ce qui n’a pas beaucoup de sens. Comment allons nous faire pour permettre aux développeurs de l’utiliser ?

Nous allons construire un pseudo constructeur. Pour cela il faut déclarer une méthode statique qui retournera un objet correspondant au type de la classe. L’avantage de cette méthode par rapport à un constructeur, est que l’on peut contrôler la valeur que l’on va retourner. Le fait que cette méthode soit déclarée statique permet de l’appeler sans posséder d’instance de cette classe. A noter que, par convention, ce pseudo constructeur est nommé getInstance.

Pour en finir avec le concept de base du Singleton voyons comment implémenter cette méthode.

Tout d’abord il faut créer un attribut statique qui va permettre de stocker l’unique instance de la classe. Ensuite, dans le pseudo constructeur on va tester cet attribut. Si celui-ci est nul alors on crée une instance de la classe et on stocke sa valeur dans cet attribut. Sinon c’est que l’attribut possède déjà une instance de la classe. Dans tous les cas la méthode retourne la valeur de l’attribut possédant l’unique instance de la classe.

## Exemple

```
class Singleton {

    // Instance unique de la classe
    private static $instance;

    // Empêche l'instanciation directe depuis l'extérieur
    private function __construct() {
        // Initialisation de la classe
    }

    // Méthode pour accéder à l'instance unique
    public static function getInstance() {
        if (!self::$instance) {
            self::$instance = new self();
        }

        return self::$instance;
    }

    // Exemple d'une méthode de la classe
    public function someMethod() {
        echo "Méthode de l'instance unique appelée.";
    }
}
```

### Conséquences

Jusqu’à maintenant nous avons passé sous silence un problème qui peut mettre en péril l’implémentation du pattern Singleton : le multithread (capacité pour un programme de lancer plusieurs traitements simultanés c'est à dire processus). En effet, si l’on implémente de façon basique le pattern Singleton, dans le cas d’un programme multithread, on peut se retrouver avec une classe Singleton possédant plusieurs instances.

Le problème réside dans l’enchaînement des instructions. Un premier processus exécute la fonction getInstance constate que l’attribut instance est nul. Un deuxième processus s’exécute et lui aussi constate (via getInstance) que l’attribut instance est nul. Il va donc créer une instance et retourner celle-ci. Lorsque le premier processus va reprendre son exécution il va à son tour créer une nouvelle instance (étant donné qu’il a déjà effectué le test sur l’attribut) et retourner celle-ci. On se retrouve alors avec deux instances pour une classe Singleton.

Pour résoudre ce problème, on peut instancier l’attribut instance dès sa déclaration dans la classe. Ainsi l’implémentation de la méthode getInstance se limite à retourner l’attribut instance. Cette approche est fiable et simple à mettre en place mais on perd l’instanciation à la demande.

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

La solution consiste à créer une série de classes, chacune implémentant la même interface que la classe de base, mais avec des fonctionnalités supplémentaires. Ces classes supplémentaires, appelées décorateurs, sont agencées en une chaîne. Chaque décorateur encapsule une instance de la classe de base et peut ajouter des comportements avant ou après l'appel à la méthode de la classe de base.

### Exemple

```
// Interface Produit
interface Produit {
    public function getDescription(): string;
    public function getPrix(): float;
}

// Classe Concrète Gaufre
class Gaufre implements Produit {
    public function getDescription(): string {
        return "Gaufre";
    }

    public function getPrix(): float {
        return 5.0;
    }
}

// Classe Concrète Crêpe
class Crepe implements Produit {
    public function getDescription(): string {
        return "Crêpe";
    }

    public function getPrix(): float {
        return 6.0;
    }
}

// Classe Décorateur Supplément
abstract class SupplementDecorator implements Produit {
    protected $produit;

    public function __construct(Produit $produit) {
        $this->produit = $produit;
    }

    public function getDescription(): string {
        return $this->produit->getDescription();
    }

    public function getPrix(): float {
        return $this->produit->getPrix();
    }
}

// Classe Décorateur Chocolat
class ChocolatDecorator extends SupplementDecorator {
    public function getDescription(): string {
        return $this->produit->getDescription() . ", Chocolat";
    }

    public function getPrix(): float {
        return $this->produit->getPrix() + 2.0;
    }
}

// Classe Décorateur Chantilly
class ChantillyDecorator extends SupplementDecorator {
    public function getDescription(): string {
        return $this->produit->getDescription() . ", Chantilly";
    }

    public function getPrix(): float {
        return $this->produit->getPrix() + 1.5;
    }
}

// Exemple d'utilisation
$gaufre = new Gaufre();
$gaufreAvecChocolat = new ChocolatDecorator($gaufre);
$gaufreAvecChocolatEtChantilly = new ChantillyDecorator($gaufreAvecChocolat);

echo "Description: " . $gaufreAvecChocolatEtChantilly->getDescription() . "\n";
echo "Prix: " . $gaufreAvecChocolatEtChantilly->getPrix() . "€\n";
```

### Conséquences

#### Avantages :

- Flexibilité : Vous pouvez ajouter ou supprimer des fonctionnalités de manière dynamique en combinant différents décorateurs.
- Réutilisabilité : Les classes de base restent inchangées, ce qui facilite la réutilisation du code existant.
- Extensibilité : Vous pouvez ajouter de nouveaux décorateurs sans modifier les classes existantes, permettant une extension facile.

#### Inconvénients :

- Complexité accrue : L'ajout de nombreux décorateurs peut rendre le code complexe, en particulier lorsque la hiérarchie de classes devient profonde.
- Redondance potentielle : Si plusieurs décorateurs ont des fonctionnalités similaires, cela peut entraîner une certaine redondance.
- Ordre des décorateurs : L'ordre dans lequel les décorateurs sont empilés peut avoir un impact sur le comportement final.

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

## Visitor : Yannick (23/23)

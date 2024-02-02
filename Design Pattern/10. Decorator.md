## Décorateur : Yannick (10/23)

> Le pattern Décorateur (Decorator) attache dynamiquement des responsabilités supplémentaires à un objet. Il fournit une alternative souple à l’héritage, pour étendre des fonctionnalités.

### Problématique

Dans la programmation orientée objet, la façon la plus classique d’ajouter des fonctionnalités à une classe est d’utiliser l’héritage. Pourtant il arrive parfois de vouloir ajouter des fonctionnalités à une classe sans utiliser l’héritage. En effet, si l’on hérite d’une classe la redéfinition d’une méthode peut entraîner l’ajout de nouveaux bugs. On peut aussi être reticent à l’idée que des méthodes de la classe mère soient appelées directement depuis notre nouvelle classe.

De plus, l’héritage doit être utilisé avec parcimonie. Car si on abuse de ce principe de la programmation orientée objet, on aboutit rapidement à un modèle complexe contenant un grand nombre de classes.

Un autre souci de l’héritage est l’ajout de fonctionnalités de façon statique. En effet, l’héritage de classe se définit lors de l’écriture du programme et ne peut être modifié après la compilation. Or, dans certains cas, on peut vouloir rajouter des fonctionnalités de façon dynamique.

D’une manière générale on constate que l’ajout de fonctionnalités dans un programme s’avère parfois délicat et complexe. Ce problème peut être résolu si le développeur a identifié, dès la conception, qu’une partie de l’application serait sujette à de fortes évolutions. Il peut alors faciliter ces modifications en utilisant le pattern Décorateur. La puissance de ce pattern qui permet d’ajouter (ou modifier) des fonctionnalités facilement provient de la combinaison de l’héritage et de la composition. Ainsi les problèmes cités ci-dessus ne se posent plus lors de l’utilisation de ce pattern.

### Solution

![Diagramme UML](https://refactoring.guru/images/patterns/diagrams/decorator/solution2.png?id=cbee4a27080ce3a0bf773482613e1347)

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
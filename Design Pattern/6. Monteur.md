# Nom
Monteur

# Problématique
Le patron de conception Monteur résout le problème de construction d'objets complexes en séparant la construction de l'objet de sa représentation. Il permet de créer différentes représentations d'un objet en utilisant le même processus de construction.

# Solution
![Diagramme UML](https://refactoring.guru/images/patterns/diagrams/builder/structure.png?id=fe9e23559923ea0657aa5fe75efef333)

Le patron de conception Monteur propose une solution en introduisant un monteur qui est responsable de la construction d'une variété d'objets complexes. Le directeur dirige le processus de construction en utilisant le monteur approprié, permettant ainsi de créer différentes représentations d'un objet complexe.

# Exemple
```
// Produit complexe à construire
class Product {
    private $part1;
    private $part2;

    public function setPart1($part1) {
        $this->part1 = $part1;
    }

    public function setPart2($part2) {
        $this->part2 = $part2;
    }

    public function getResult() {
        return "Part1: " . $this->part1 . " | Part2: " . $this->part2;
    }
}

// Monteur abstrait avec les étapes de construction
interface Builder {
    public function buildPart1();
    public function buildPart2();
    public function getResult();
}

// Monteur concret qui implémente les étapes de construction
class ConcreteBuilder implements Builder {
    private $product;

    public function __construct() {
        $this->product = new Product();
    }

    public function buildPart1() {
        $this->product->setPart1("Part1 built");
    }

    public function buildPart2() {
        $this->product->setPart2("Part2 built");
    }

    public function getResult() {
        return $this->product;
    }
}

// Directeur qui supervise le processus de construction
class Director {
    private $builder;

    public function setBuilder(Builder $builder) {
        $this->builder = $builder;
    }

    public function construct() {
        $this->builder->buildPart1();
        $this->builder->buildPart2();
    }
}

// Client utilisant le monteur pour construire un produit complexe
function clientCode(Director $director, Builder $builder) {
    $director->setBuilder($builder);
    $director->construct();

    return $builder->getResult()->getResult();
}

// Utilisation du code avec le monteur concret
$builder = new ConcreteBuilder();
$director = new Director();

echo clientCode($director, $builder);
```


# Conséquences

Avantages :
- Principe de responsabilité unique. Le processus de construction est délégué au monteur, isolant ainsi le code client de la complexité de la construction.
- Principe ouvert/fermé. Possibilité d'introduire de nouveaux monteurs pour créer différentes représentations de produits sans altérer le code client existant. Ces nouveaux monteurs doivent respecter l'interface du monteur abstrait.

Inconvénients :
- La complexité générale du code augmente, car il est nécessaire de créer de nouvelles classes pour chaque variation de produit. Cependant, cela permet une meilleure gestion de la création d'objets complexes avec des représentations différentes. 
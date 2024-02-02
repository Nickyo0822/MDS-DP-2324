# Nom :
Fabrique Abstraite

# Problématique :
Le patron de conception Fabrique Abstraite résout le problème de création d'ensembles d'objets liés ou dépendants sans spécifier leurs classes concrètes. Il offre une interface pour la création de familles d'objets liés ou dépendants, sans préciser leurs classes concrètes.

# Solution :

![Diagramme UML](https://refactoring.guru/images/patterns/diagrams/abstract-factory/structure.png?id=a3112cdd98765406af94595a3c5e7762)

Le patron de conception Fabrique Abstraite propose une solution en introduisant une interface abstraite (fabrique) qui définit les méthodes de création pour chaque type d'objet. Les classes concrètes de la fabrique implémentent ces méthodes pour créer des objets cohérents entre eux.

```
// Interface abstraite pour la création de produits de type A
interface AbstractProductA {
    public function operationA();
}

// Interface abstraite pour la création de produits de type B
interface AbstractProductB {
    public function operationB();
}

// Implémentation concrète de la fabrique abstraite
class ConcreteFactory implements AbstractFactory {
    public function createProductA(): AbstractProductA {
        return new ConcreteProductA();
    }

    public function createProductB(): AbstractProductB {
        return new ConcreteProductB();
    }
}

// Implémentation concrète de produit de type A
class ConcreteProductA implements AbstractProductA {
    public function operationA() {
        return "Opération A";
    }
}

// Implémentation concrète de produit de type B
class ConcreteProductB implements AbstractProductB {
    public function operationB() {
        return "Opération B";
    }
}

// Client utilisant la fabrique abstraite
function clientCode(AbstractFactory $factory) {
    $productA = $factory->createProductA();
    $productB = $factory->createProductB();

    return $productA->operationA() . " | " . $productB->operationB();
}

// Utilisation du code avec la fabrique concrète
$factory = new ConcreteFactory();
echo clientCode($factory);
```

# Conséquences :
Avantages :
- Principe de responsabilité unique. La création d'objets est déléguée à la fabrique, isolant ainsi le code de création du reste du programme.
- Principe ouvert/fermé. Possibilité d'introduire de nouveaux ensembles de produits en créant de nouvelles classes de fabrique sans altérer le code client existant. Ces nouvelles fabriques doivent respecter l'interface abstraite du client.

Inconvénients :
- La complexité générale du code augmente en introduisant de nouvelles classes et interfaces. Cependant, cela permet une meilleure gestion de l'évolution des ensembles d'objets liés.
# Nom
Façade

# Problématique
Le patron de conception Facade résout le problème de complexité en fournissant une interface unifiée pour un ensemble d'interfaces dans un sous-système.

# Solution
![Diagramme UML](https://refactoring.guru/images/patterns/diagrams/facade/structure.png?id=258401362234ac77a2aaf1cde62339e7)

Le patron de conception Facade propose une solution en introduisant une classe Facade qui offre une interface simplifiée pour interagir avec un ensemble de classes complexes du sous-système. Elle agit comme une interface unifiée, masquant la complexité du sous-système.

# Exemple

```
// Sous-système complexe avec plusieurs classes
class SubsystemA {
    public function operationA() {
        return "Subsystem A Operation";
    }
}

class SubsystemB {
    public function operationB() {
        return "Subsystem B Operation";
    }
}

class SubsystemC {
    public function operationC() {
        return "Subsystem C Operation";
    }
}

// Facade fournissant une interface unifiée
class Facade {
    private $subsystemA;
    private $subsystemB;
    private $subsystemC;

    public function __construct() {
        $this->subsystemA = new SubsystemA();
        $this->subsystemB = new SubsystemB();
        $this->subsystemC = new SubsystemC();
    }

    public function operation() {
        $result = "Facade Operation:\n";
        $result .= $this->subsystemA->operationA() . "\n";
        $result .= $this->subsystemB->operationB() . "\n";
        $result .= $this->subsystemC->operationC() . "\n";
        return $result;
    }
}

// Client utilisant la façade pour interagir avec le sous-système
function clientCode(Facade $facade) {
    return $facade->operation();
}

// Utilisation du code avec la façade
$facade = new Facade();
echo clientCode($facade);
```

# Conséquences

Avantages :
- Simplification de l'interface. La façade offre une interface unifiée, facilitant l'utilisation du sous-système sans avoir à connaître sa complexité interne.
- Principe de responsabilité unique. La façade isole le code client de la complexité du sous-système en fournissant une interface simplifiée.
- Principe ouvert/fermé. Possibilité de modifier le sous-système sans impacter le code client, tant que l'interface de la façade reste inchangée.

Inconvénients :
- La façade peut devenir un point de surcharge s'il doit supporter de nombreuses opérations différentes. Cependant, c'est souvent un compromis acceptable pour simplifier l'utilisation du sous-système.
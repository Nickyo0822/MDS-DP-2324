# Nom
Composite

# Problématique
Le patron de conception Composite résout le problème de traitement uniforme d'objets individuels et de compositions d'objets. Il permet de traiter de manière homogène les objets simples et les structures d'objets complexes.


# Solution
![Diagramme-uml](https://refactoring.guru/images/patterns/diagrams/composite/structure-fr.png?id=e41692527596ede08b8ab82668956e99)

Le patron de conception Composite propose une solution en définissant une interface commune pour les objets simples et les composites. Les objets individuels et les composites implémentent cette interface, permettant ainsi un traitement uniforme dans toute la structure.

# Exemple
```
// Interface commune pour les objets simples et les composites
interface Component {
    public function operation();
}

// Implémentation concrète pour les objets simples
class Leaf implements Component {
    private $name;

    public function __construct($name) {
        $this->name = $name;
    }

    public function operation() {
        return "Leaf: " . $this->name;
    }
}

// Implémentation concrète pour les composites
class Composite implements Component {
    private $children;

    public function __construct() {
        $this->children = [];
    }

    public function add(Component $component) {
        $this->children[] = $component;
    }

    public function operation() {
        $result = "Composite: ";
        foreach ($this->children as $child) {
            $result .= $child->operation() . " ";
        }
        return $result;
    }
}

// Client traitant uniformément les objets simples et les composites
function clientCode(Component $component) {
    return $component->operation();
}

// Utilisation du code avec des objets simples et des composites
$leaf1 = new Leaf("Leaf 1");
$leaf2 = new Leaf("Leaf 2");
$composite = new Composite();
$composite->add($leaf1);
$composite->add($leaf2);

echo clientCode($composite);
```

# Conséquences

Avantages :
- Principe de responsabilité unique. Les objets simples et les composites partagent une interface commune, permettant un traitement uniforme dans le code client.
- Principe ouvert/fermé. Possibilité d'ajouter de nouveaux types d'objets simples ou de composites sans altérer le code client existant. Ces nouveaux éléments doivent respecter l'interface commune.

Inconvénients :
- La complexité générale du code augmente légèrement. 
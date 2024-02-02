# Nom
Memento

# Problématique
![Diagramme UML](https://refactoring.guru/images/patterns/diagrams/memento/solution-fr.png?id=61e190b167107ad4f8c74b29f9fb7b22)

Le patron de conception Memento résout le problème de la capture et de la restauration de l'état interne d'un objet sans révéler sa structure interne. Il permet de sauvegarder et de restaurer l'état d'un objet à un moment donné.

# Solution

Le patron de conception Memento propose une solution en introduisant trois principaux acteurs : l'originateur, le memento et le gardien. L'originateur crée un memento pour sauvegarder son état interne, et le gardien est responsable de la gestion des mementos.

# Exemple

```
// Originateur qui crée un memento pour sauvegarder son état interne
class Originator {
    private $state;

    public function setState($state) {
        $this->state = $state;
    }

    public function getState() {
        return $this->state;
    }

    public function createMemento() {
        return new Memento($this->state);
    }

    public function restoreMemento(Memento $memento) {
        $this->state = $memento->getState();
    }
}

// Memento qui sauvegarde l'état interne de l'originateur
class Memento {
    private $state;

    public function __construct($state) {
        $this->state = $state;
    }

    public function getState() {
        return $this->state;
    }
}

// Gardien qui gère les mementos
class Caretaker {
    private $mementos;

    public function addMemento(Memento $memento) {
        $this->mementos[] = $memento;
    }

    public function getMemento($index) {
        return $this->mementos[$index];
    }
}

// Client utilisant l'originateur, le memento et le gardien
function clientCode() {
    $originator = new Originator();
    $caretaker = new Caretaker();

    // Modification de l'état de l'originateur
    $originator->setState("State 1");

    // Sauvegarde de l'état actuel
    $caretaker->addMemento($originator->createMemento());

    // Modification de l'état de l'originateur
    $originator->setState("State 2");

    // Restauration de l'état précédent
    $originator->restoreMemento($caretaker->getMemento(0));

    return $originator->getState();
}

// Utilisation du code avec le patron de conception Memento
echo clientCode();

```


# Conséquences
Avantages :
- Principe de responsabilité unique. L'originateur est responsable de sa propre sauvegarde, déchargeant le client de cette responsabilité.
- Principe ouvert/fermé. Possibilité d'ajouter de nouvelles fonctionnalités de sauvegarde et de restauration sans altérer l'originateur.

Inconvénients :
- La gestion des mementos peut devenir complexe si l'état interne de l'originateur est volumineux. Cependant, cela peut être atténué en ne sauvegardant que les informations cruciales.
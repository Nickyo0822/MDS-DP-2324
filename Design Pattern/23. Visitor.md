## Visitor : Yannick (23/23)

> Le design pattern Visitor permet de définir de nouvelles opérations sur une structure d'objets sans modifier ces objets.

### Problématique

Le design pattern Visitor résout le problème d'ajouter de nouvelles opérations à une structure d'objets sans modifier les classes de ces objets. Il est particulièrement utile lorsque vous avez une hiérarchie d'objets avec différentes classes, et que vous souhaitez effectuer diverses opérations spécifiques à chacune de ces classes sans modifier ces classes elles-mêmes.

### Solution

![Diagramme UML](https://refactoring.guru/images/patterns/diagrams/visitor/example.png?id=d66acd1b9096c47db17ab3bb82b54a59)

Le design pattern Visitor consiste à définir une interface commune appelée "Visiteur" avec des méthodes spécifiques pour chaque type d'objet à visiter. Chaque classe concrète d'objet à visiter implémente une méthode accepter qui prend en paramètre un objet visiteur. L'objet visiteur, à son tour, implémente des méthodes spécifiques pour chaque type d'objet visité. Ainsi, les objets acceptent les visiteurs, permettant aux visiteurs d'accéder à leurs propriétés et d'effectuer des opérations spécifiques.

### Exemple

```
// Interface Visiteur
interface Visiteur {
    public function visiterCercle(Cercle $cercle);
    public function visiterCarre(Carre $carre);
}

// Interface Element
interface Element {
    public function accepter(Visiteur $visiteur);
}

// Classe Concrète Cercle
class Cercle implements Element {
    public $rayon;

    public function __construct($rayon) {
        $this->rayon = $rayon;
    }

    public function accepter(Visiteur $visiteur) {
        $visiteur->visiterCercle($this);
    }
}

// Classe Concrète Carré
class Carre implements Element {
    public $cote;

    public function __construct($cote) {
        $this->cote = $cote;
    }

    public function accepter(Visiteur $visiteur) {
        $visiteur->visiterCarre($this);
    }
}

// Classe Concrète d'Opération (Visiteur)
class CalculStatistiqueVisiteur implements Visiteur {
    public $aireTotale = 0;
    public $circonferenceTotale = 0;

    public function visiterCercle(Cercle $cercle) {
        $this->aireTotale += pi() * pow($cercle->rayon, 2);
        $this->circonferenceTotale += 2 * pi() * $cercle->rayon;
    }

    public function visiterCarre(Carre $carre) {
        $this->aireTotale += pow($carre->cote, 2);
        $this->circonferenceTotale += 4 * $carre->cote;
    }
}

// Utilisation
$cercle = new Cercle(5);
$carre = new Carre(4);

$calculateur = new CalculStatistiqueVisiteur();

$cercle->accepter($calculateur);
$carre->accepter($calculateur);

echo "Aire totale : " . $calculateur->aireTotale . "\n";
echo "Circonférence totale : " . $calculateur->circonferenceTotale . "\n";
```

### Conséquences

#### Avantages :

- Séparation des préoccupations : Le design pattern Visitor sépare les opérations des classes d'objets, ce qui rend le code plus modulaire et extensible.
- Facilité d'ajout de nouvelles opérations : Vous pouvez ajouter de nouvelles opérations sans modifier les classes existantes, facilitant l'extension du code.

#### Inconvénients :

- Complexité accrue : L'introduction de visiteurs et de nouvelles interfaces peut augmenter la complexité du code.
- Difficulté à maintenir : Si de nouveaux types d'objets sont fréquemment ajoutés, la maintenance du code peut devenir plus complexe.
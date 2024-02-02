## Pont : Yannick (5/23)

> Le design pattern Pont permet de séparer une abstraction de son implémentation, permettant à ces deux aspects d'évoluer indépendamment.

### Problématique

Le design pattern Pont résout le problème de la séparation entre une abstraction et son implémentation, permettant à ces deux aspects de varier indépendamment. Il est utilisé lorsqu'on veut éviter une liaison permanente entre une abstraction (interface ou classe abstraite) et son implémentation concrète, permettant ainsi une évolutivité plus flexible.

### Solution

![Diagramme UML](https://refactoring.guru/images/patterns/diagrams/bridge/structure-en.png?id=827afa4b40008dc29d26fe0f4d41b9cc)

Le design pattern Pont utilise une structure de type "pont" où une interface (abstraction) possède une référence à une autre interface (implémentation). Cela permet à l'abstraction de déléguer l'implémentation à une classe concrète tout en restant indépendante des détails spécifiques de cette implémentation.

### Exemple

```
// Interface Implementor (Dessinateur)
interface Dessinateur {
    public function dessinerCercle($rayon);
    public function dessinerCarre($cote);
}

// Implementor Concret A (DessinateurA)
class DessinateurA implements Dessinateur {
    public function dessinerCercle($rayon) {
        echo "Dessinateur A - Cercle de rayon $rayon\n";
    }

    public function dessinerCarre($cote) {
        echo "Dessinateur A - Carré de côté $cote\n";
    }
}

// Implementor Concret B (DessinateurB)
class DessinateurB implements Dessinateur {
    public function dessinerCercle($rayon) {
        echo "Dessinateur B - Cercle de rayon $rayon\n";
    }

    public function dessinerCarre($cote) {
        echo "Dessinateur B - Carré de côté $cote\n";
    }
}

// Abstraction (Forme)
abstract class Forme {
    protected $dessinateur;

    public function __construct(Dessinateur $dessinateur) {
        $this->dessinateur = $dessinateur;
    }

    abstract public function dessiner();
}

// Raffinement Abstraction A (FormeCercle)
class FormeCercle extends Forme {
    private $rayon;

    public function __construct(Dessinateur $dessinateur, $rayon) {
        parent::__construct($dessinateur);
        $this->rayon = $rayon;
    }

    public function dessiner() {
        $this->dessinateur->dessinerCercle($this->rayon);
    }
}

// Raffinement Abstraction B (FormeCarre)
class FormeCarre extends Forme {
    private $cote;

    public function __construct(Dessinateur $dessinateur, $cote) {
        parent::__construct($dessinateur);
        $this->cote = $cote;
    }

    public function dessiner() {
        $this->dessinateur->dessinerCarre($this->cote);
    }
}

// Utilisation
$dessinateurA = new DessinateurA();
$dessinateurB = new DessinateurB();

$formeCercleA = new FormeCercle($dessinateurA, 5);
$formeCarreB = new FormeCarre($dessinateurB, 4);

$formeCercleA->dessiner();
$formeCarreB->dessiner();g
```

### Conséquences

#### Avantages :

- Séparation des préoccupations : Le design pattern Pont permet de séparer l'abstraction de son implémentation, facilitant la gestion des variations dans chaque aspect.
- Flexibilité : Les classes d'abstraction et d'implémentation peuvent évoluer indépendamment, permettant une flexibilité dans la conception.
- Réduction de la duplication de code : Les différentes implémentations peuvent réutiliser le même code d'abstraction.

#### Inconvénients :

- Complexité accrue : L'introduction du pont peut augmenter la complexité du code, en particulier pour les petites applications.
- Plusieurs hiérarchies : Si mal utilisé, cela peut entraîner la création de plusieurs hiérarchies de classes, rendant le code difficile à comprendre.
# Nom : 
Strategy 

# Problématique : 
La Stratégie (ou Strategy en anglais) est un patron de conception comportemental qui est utile lorsque vous avez plusieurs façons de faire quelque chose, et que vous souhaitez pouvoir choisir entre ces façons de façon flexible sans changer le code principal. Cela vous permet de séparer le comportement de votre programme en différentes stratégies interchangeables.

# Solution : 
![Diagramme UML](https://commons.wikimedia.org/wiki/File:StrategyPattern_IBrakeBehavior.svg#/media/File:StrategyPattern_IBrakeBehavior.svg)

Dans le patron de conception Stratégie, vous créez une interface commune pour toutes les stratégies possibles, décrivant ainsi un ensemble de comportements interchangeables. Ensuite, vous implémentez chaque stratégie spécifique en créant des classes qui implémentent cette interface. Enfin, dans votre programme principal, vous instanciez la stratégie appropriée et l'injectez dans le contexte où le comportement est nécessaire, permettant ainsi une gestion flexible des algorithmes.

```
interface IBrakeBehavior {
    public function brake();
}

class BrakeWithABS implements IBrakeBehavior {
    public function brake() {
        echo "Brake with ABS applied\n";
    }
}

class BrakeNoABS implements IBrakeBehavior {
    public function brake() {
        echo "Simple Brake applied\n";
    }
}

abstract class Car {
    private $brakeBehavior;

    public function __construct(IBrakeBehavior $brakeBehavior) {
        $this->brakeBehavior = $brakeBehavior;
    }

    public function applyBrake() {
        $this->brakeBehavior->brake();
    }

    public function setBrakeBehavior(IBrakeBehavior $brakeType) {
        $this->brakeBehavior = $brakeType;
    }
}

class Sedan extends Car {
    public function __construct() {
        parent::__construct(new BrakeNoABS());
    }
}

class SUV extends Car {
    public function __construct() {
        parent::__construct(new BrakeWithABS());
    }
}

$sedanCar = new Sedan();
$sedanCar->applyBrake();

$suvCar = new SUV();
$suvCar->applyBrake();

$suvCar->setBrakeBehavior(new BrakeNoABS());
$suvCar->applyBrake();
```

# Conséquences :

- Avantages : 
1) Vous pouvez permuter l'algorithme utilisé à l'intérieur d'un objet à l'exécution
2) Vous pouvez séparer les détails de l'implémentation d'un algorithme et le code qui l'utilise
3) Vous pouvez remplacer l'héritage par la composition
4) Principe ouvert/fermé. Possibilité d'ajouter de nouvelles stratégies sans avoir à modifier le contexte

- Inconvénients :  
1) Les clients doivent pouvoir comparer les différentes stratégies et choisir la bonne
2) Si il n'y a que quelques algorithmes qui ne varient pas beaucoup, pas besoin de rendre le programme plus compliqué avec les nouvelles classes et interfaces qui accompagnent la mise en place du patron
3) Dans de nombreux langages de programmations, vous pouvez utiliser des fonctions anonymes pour implémenter différentes versions d'un algorithme. Ces fonctions peuvent être utilisées de manière similaire aux objets stratégie, sans avoir besoin de créer des classes et des interfaces supplémentaires, ce qui rend votre code plus concis et facile à comprendre
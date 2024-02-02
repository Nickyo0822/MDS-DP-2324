# Nom : 
Mediator 

# Problématique : 
Le patron de conception Mediateur (ou Mediator en anglais) est utilisé pour faciliter la communication entre les objets en les déconnectant directement les uns des autres. Il permet de réduire les dépendances directes entre les composants en introduisant un intermédiaire central qui coordonne les intéractions entre eux.

# Solution : 
![Diagramme UML](https://refactoring.guru/images/patterns/diagrams/mediator/structure.png)

Le patron Mediateur implémente un objet médiateur qui agit comme un point central de communication entre différents objets. Plutôt que de permettre à ces objets de communiquer directement entre eux, le médiateur contrôle les échanges en les routant à travers lui, favorisant ainsi un couplage plus faible et une meilleure maintenabilité du système.

```
namespace RefactoringGuru\Mediator\Conceptual;

interface Mediator
{
    public function notify(object $sender, string $event): void;
}

class ConcreteMediator implements Mediator
{
    private $component1;
    private $component2;

    public function __construct(Component1 $c1, Component2 $c2)
    {
        $this->component1 = $c1;
        $this->component1->setMediator($this);
        $this->component2 = $c2;
        $this->component2->setMediator($this);
    }

    public function notify(object $sender, string $event): void
    {
        if ($event == "A") {
            echo "Mediator reacts on A and triggers following operations:\n";
            $this->component2->doC();
        }

        if ($event == "D") {
            echo "Mediator reacts on D and triggers following operations:\n";
            $this->component1->doB();
            $this->component2->doC();
        }
    }
}

class BaseComponent
{
    protected $mediator;

    public function __construct(Mediator $mediator = null)
    {
        $this->mediator = $mediator;
    }

    public function setMediator(Mediator $mediator): void
    {
        $this->mediator = $mediator;
    }
}

class Component1 extends BaseComponent
{
    public function doA(): void
    {
        echo "Component 1 does A.\n";
        $this->mediator->notify($this, "A");
    }

    public function doB(): void
    {
        echo "Component 1 does B.\n";
        $this->mediator->notify($this, "B");
    }
}

class Component2 extends BaseComponent
{
    public function doC(): void
    {
        echo "Component 2 does C.\n";
        $this->mediator->notify($this, "C");
    }

    public function doD(): void
    {
        echo "Component 2 does D.\n";
        $this->mediator->notify($this, "D");
    }
}

$c1 = new Component1();
$c2 = new Component2();
$mediator = new ConcreteMediator($c1, $c2);

echo "Client triggers operation A.\n";
$c1->doA();

echo "Client triggers operation D.\n";
$c2->doD();
```

# Conséquences : 

- Avantages :
1) Principe de responsabilité unique. Vous pouvez mettre les communications entre les différents composants au même endroit, rendant le code plus facile à comprendre et à maintenir
2) Principe ouvert/fermé. Vous pouvez ajouter de nouveaux médiateurs sans avoir à modifier les composants déjà en place
3) Vous diminuez le couplage entre les différents composants d'un programme
4) Vous pouvez réutiliser les composants individuels plus facilement
- Inconvénients : Avec le temps, un médiateur peut évoluer en Objet Omniscient (un objet qui reconnaît trop de choses ou fait trop de choses)
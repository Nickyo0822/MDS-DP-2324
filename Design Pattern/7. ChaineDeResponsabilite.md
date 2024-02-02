# Nom : 
Chaîne de responsabilité 

# Problématique : 
Le patron de conception Chaîne de responsabilité (ou Chain of responsibility en anglais) est utile lorsque l'on a une série d'objet qui peuvent traiter une demande, mais qu'on ne sait pas à l'avance lequel le fera. Il permet de passer la demande le long d'une chaîne d'objets jusqu'à ce qu'elle soit traitée, offrant ainsi une flexibilité pour ajouter, supprimer ou reconfigurer les traitements sans changer le code client.

# Solution : 
![Diagramme UML](https://refactoring.guru/images/patterns/diagrams/chain-of-responsibility/structure.png)

Le patron de conception Chaine de responsabilité est une solution qui consiste à organiser les objets en une séquence, où chaque objet peut traiter une requête ou la transmettre au suivant de la chaîne. Lorsqu'une demande est émise, elle est acheminée à travers les objets de la chaîne jusqu'à ce qu'un objet puisse la traiter. Cela permet un découpage flexible entre l'émetteur de la demande et les objets de traitement, facilitant l'ajout, la suppression ou la réorganisation des traitements sans altérer le code source client.

```
namespace RefactoringGuru\ChainOfResponsibility\Conceptual;

/**
 * The Handler interface declares a method for building the chain of handlers.
 * It also declares a method for executing a request.
 */
interface Handler
{
    public function setNext(Handler $handler): Handler;

    public function handle(string $request): ?string;
}

/**
 * The default chaining behavior can be implemented inside a base handler class.
 */
abstract class AbstractHandler implements Handler
{
    /**
     * @var Handler
     */
    private $nextHandler;

    public function setNext(Handler $handler): Handler
    {
        $this->nextHandler = $handler;
        // Returning a handler from here will let us link handlers in a
        // convenient way like this:
        // $monkey->setNext($squirrel)->setNext($dog);
        return $handler;
    }

    public function handle(string $request): ?string
    {
        if ($this->nextHandler) {
            return $this->nextHandler->handle($request);
        }

        return null;
    }
}

/**
 * All Concrete Handlers either handle a request or pass it to the next handler
 * in the chain.
 */
class MonkeyHandler extends AbstractHandler
{
    public function handle(string $request): ?string
    {
        if ($request === "Banana") {
            return "Monkey: I'll eat the " . $request . ".\n";
        } else {
            return parent::handle($request);
        }
    }
}

class SquirrelHandler extends AbstractHandler
{
    public function handle(string $request): ?string
    {
        if ($request === "Nut") {
            return "Squirrel: I'll eat the " . $request . ".\n";
        } else {
            return parent::handle($request);
        }
    }
}

class DogHandler extends AbstractHandler
{
    public function handle(string $request): ?string
    {
        if ($request === "MeatBall") {
            return "Dog: I'll eat the " . $request . ".\n";
        } else {
            return parent::handle($request);
        }
    }
}

/**
 * The client code is usually suited to work with a single handler. In most
 * cases, it is not even aware that the handler is part of a chain.
 */
function clientCode(Handler $handler)
{
    foreach (["Nut", "Banana", "Cup of coffee"] as $food) {
        echo "Client: Who wants a " . $food . "?\n";
        $result = $handler->handle($food);
        if ($result) {
            echo "  " . $result;
        } else {
            echo "  " . $food . " was left untouched.\n";
        }
    }
}

/**
 * The other part of the client code constructs the actual chain.
 */
$monkey = new MonkeyHandler();
$squirrel = new SquirrelHandler();
$dog = new DogHandler();

$monkey->setNext($squirrel)->setNext($dog);

/**
 * The client should be able to send a request to any handler, not just the
 * first one in the chain.
 */
echo "Chain: Monkey > Squirrel > Dog\n\n";
clientCode($monkey);
echo "\n";

echo "Subchain: Squirrel > Dog\n\n";
clientCode($squirrel);
```

# Conséquences : 

- Avantages : 
1) Possibilité de contrôler l'ordre des traitements de la demande.
2) Principe de responsabilité unique. Possibilité de découpler les classes qui appellent des traitements, de celles qui les exécutent.
3) Principe ouvert/fermé. Possibilité d'ajouter de nouveaux handlers dans le programme sans toucher au code client existant.
- Inconvénients : Il se peut que certaines demandes ne soient pas traitées.
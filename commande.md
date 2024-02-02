## Nom Command

### Problématique :

La problématique que résout le design pattern Command se présente lorsque vous avez besoin de découpler l'émetteur d'une demande (le client) du destinataire de cette demande (le receveur), tout en permettant la paramétrisation de ces demandes. En d'autres termes, comment encapsuler une requête sous la forme d'un objet, de manière à ce que le client puisse spécifier des requêtes sans connaître les détails du traitement côté receveur, et à permettre l'annulation des opérations.

### Solution :

Command propose de créer un objet Command pour chaque demande, contenant à la fois l'action à exécuter (sous la forme d'une méthode) et tous les paramètres nécessaires à son exécution. Ces objets Command sont ensuite associés à des classes concrètes, appelées Commandes concrètes, qui implémentent l'interface Command. Cette approche permet d'encapsuler les demandes et de les rendre interchangeables, sans que le client n'ait besoin de connaître les détails de leur implémentation.

La classe client crée un objet Command avec les paramètres appropriés, puis l'associe à un objet qui sait comment exécuter cette commande. Le client n'a pas besoin de savoir comment l'action sera réalisée ni comment elle sera traitée côté receveur. En outre, cette structure offre la possibilité d'annuler les opérations en implémentant une méthode Undo dans les objets Command, permettant de revenir à l'état précédent du système.

### Conséquences :

Les avantages incluent la réduction du couplage entre le client et le receveur, la possibilité d'ajouter de nouvelles commandes sans modifier le code client, et la facilité de mettre en œuvre des fonctionnalités telles que l'annulation des opérations. Cependant, cela peut entraîner une prolifération des classes Command, et la nécessité d'implémenter la méthode Undo peut rendre la gestion des états plus complexe.

### Exemple (python):

```
//Interface Command
interface Command {
    execute(): void;
    undo(): void;
}

//Commande concrète pour allumer la lumière
class LightOnCommand implements Command {
    private light: Light;

    constructor(light: Light) {
        this.light = light;
    }

    execute(): void {
        this.light.turnOn();
    }

    undo(): void {
        this.light.turnOff();
    }
}

//Commande concrète pour éteindre la lumière
class LightOffCommand implements Command {
    private light: Light;

    constructor(light: Light) {
        this.light = light;
    }

    execute(): void {
        this.light.turnOff();
    }

    undo(): void {
        this.light.turnOn();
    }
}

//Receveur
class Light {
    turnOn(): void {
        // Logique pour allumer la lumière
    }

    turnOff(): void {
        // Logique pour éteindre la lumière
    }
}

//Client
class RemoteControl {
    private command: Command;

    setCommand(command: Command): void {
        this.command = command;
    }

    pressButton(): void {
        this.command.execute();
    }
}

light = new Light();
lightOnCommand = new LightOnCommand(light);
remoteControl = new RemoteControl();
remoteControl.setCommand(lightOnCommand);
remoteControl.pressButton();  # Allume la lumière
remoteControl.pressButton();  # Éteint la lumière (undo)
 

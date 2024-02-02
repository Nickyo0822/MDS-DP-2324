## Nom State

### Problématique :

Le modèle State est étroitement lié au concept d'une Machine state Fini.

L'idée fondamentale repose sur le fait qu'à tout moment, un programme se trouve dans un nombre fini d'états distincts, chacun entraînant un comportement spécifique. Le passage d'un état à un autre peut s'effectuer instantanément, mais les transitions possibles dépendent de l'état actuel, suivant des règles prédéfinies et finies.

Cette approche peut également être appliquée aux objets. Prenons l'exemple d'une classe Document. Un document peut se trouver dans l'un des trois états : Brouillon, Modération et Publié. La méthode "publier" du document agit de manière différente selon l'état actuel :

En mode Draft, elle déplace le document en modération.
En mode Moderation, elle rend le document public, mais seulement si l'utilisateur actuel est un administrateur.
En mode Published, elle ne fait rien.
Les machines d'état sont souvent mises en œuvre à l'aide d'instructions conditionnelles (if ou switch) qui sélectionnent le comportement approprié en fonction de l'état actuel de l'objet. Typiquement, cet "état" correspond simplement à un ensemble de valeurs des champs de l'objet. Même si vous n'avez jamais travaillé explicitement avec des machines à états finis, il est probable que vous ayez déjà implémenté un concept similaire. Le code suivant vous semble-t-il familier ?

La principale faiblesse d'une machine d'état basée sur des conditions devient apparente lorsque de plus en plus d'états et de comportements dépendants de l'état sont ajoutés à la classe Document. La plupart des méthodes contiennent des conditions complexes qui déterminent le comportement en fonction de l'état actuel. Ce type de code devient rapidement difficile à maintenir, car toute modification de la logique de transition peut nécessiter des modifications dans chaque méthode impliquée.

Ce problème tend à s'aggraver avec l'évolution du projet, car il est souvent difficile de prédire tous les états et transitions possibles dès le stade de la conception. Ainsi, une machine d'état initialement simple, construite avec un ensemble limité de conditions, peut se transformer en un code complexe et difficile à gérer au fil du temps.

### Solution :

Le modèle state suggère la création de nouvelles classes pour chaque état possible d'un objet, où tous les comportements spécifiques à un état donné sont encapsulés dans ces classes dédiées.

Plutôt que d'implémenter directement tous les comportements dans l'objet original, également appelé le contexte, celui-ci conserve une référence à l'un des objets d'état qui représente son état actuel. Toutes les responsabilités liées à l'état sont déléguées à cet objet d'état.

Pour changer l'état du contexte, on remplace l'objet d'état actif par un autre qui représente le nouvel état. Cette transition est possible seulement si toutes les classes d'état suivent la même interface et que le contexte interagit avec ces objets par l'intermédiaire de cette interface commune.

Cette structure peut ressembler au pattern Stratégie, mais il existe une différence clé. Dans le modèle d'État, les états particuliers peuvent être conscients les uns des autres et initier des transitions d'un état à un autre. En revanche, dans les stratégies, chaque stratégie est généralement isolée, sans connaissance mutuelle entre elles.

![Diagramme UML](https://refactoring.guru/images/patterns/diagrams/state/structure-en-indexed.png?id=303874f78151b9aebdc585080e98d773)

### Exemple :
```
class EtatCafe:
    def inserer_monnaie(self):
        pass

    def selectionner_boisson(self):
        pass

    def distribuer_boisson(self):
        pass

// Sans Monnaie
class SansMonnaie(EtatCafe):
    def inserer_monnaie(self):
        print("Monnaie insérée. Passer à l'état Monnaie Insérée.")
        return MonnaieInserée()

    def selectionner_boisson(self):
        print("Veuillez insérer de la monnaie d'abord.")

    def distribuer_boisson(self):
        print("Veuillez insérer de la monnaie d'abord.")

// Monnaie Insérée
class MonnaieInserée(EtatCafe):
    def inserer_monnaie(self):
        print("La monnaie est déjà insérée.")

    def selectionner_boisson(self):
        print("Boisson sélectionnée. Passer à l'état Distribution de Boisson.")
        return DistributionBoisson()

    def distribuer_boisson(self):
        print("Veuillez d'abord sélectionner une boisson.")

// Distribution de Boisson
class DistributionBoisson(EtatCafe):
    def inserer_monnaie(self):
        print("Impossible d'insérer de la monnaie pendant la distribution.")

    def selectionner_boisson(self):
        print("Impossible de sélectionner une nouvelle boisson pendant la distribution.")

    def distribuer_boisson(self):
        print("Distribution de la boisson en cours. Retour à l'état Sans Monnaie.")
        return SansMonnaie()

// Machine à Café
class MachineCafe:
    def __init__(self):
        self.etat_actuel = SansMonnaie()

    def changer_etat(self, nouvel_etat):
        self.etat_actuel = nouvel_etat

    def inserer_monnaie(self):
        self.etat_actuel = self.etat_actuel.inserer_monnaie()

    def selectionner_boisson(self):
        self.etat_actuel.selectionner_boisson()

    def distribuer_boisson(self):
        self.etat_actuel.distribuer_boisson()

machine_cafe = MachineCafe()
machine_cafe.inserer_monnaie()
machine_cafe.selectionner_boisson()
machine_cafe.distribuer_boisson()
```

### Conséquences du Design Pattern État :

Encapsulation des États : Le design pattern État permet d'encapsuler chaque état possible dans une classe distincte, facilitant ainsi la gestion et la maintenance du code.

Dépendance Réduite : Le contexte ne dépend plus directement de tous les comportements liés à différents états, ce qui réduit la dépendance entre le contexte et les états spécifiques.

Ajout Facile de Nouveaux États : L'ajout de nouveaux états se fait de manière aisée en introduisant de nouvelles classes d'état, sans modifier le code du contexte.

Clarté et Lisibilité : Le code devient plus lisible et compréhensible car chaque classe d'état contient uniquement les méthodes liées à cet état particulier, évitant ainsi des conditions complexes dans le code du contexte.

Flexibilité : Les transitions entre les états peuvent être gérées de manière flexible, permettant au contexte de passer d'un état à un autre en fonction de certaines conditions.

Facilité d'Évolution : L'ajout de nouveaux états ou la modification du comportement des états existants peut être effectué sans impacter le reste du code, ce qui facilite l'évolution du système.
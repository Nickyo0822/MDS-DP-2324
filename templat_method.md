## Nom Template method :

### Problématique :

Imaginons la création d'une application de data mining qui analyse les documents d'une entreprise, où les utilisateurs fournissent des fichiers de différents formats (PDF, DOC, CSV), et l'application tente de récupérer les données utiles dans un format uniforme. La première version traitait uniquement les fichiers DOC, puis les fichiers CSV ont été ajoutés, suivi des fichiers PDF. Les classes spécifiques à chaque format partagent une quantité importante de code similaire, en particulier pour le traitement et l'analyse des données. Le code client présente également des blocs conditionnels massifs qui choisissent le comportement en fonction de la classe de l'objet traité.

### Solution :

Le patron de méthode propose de découper un algorithme en une série d'étapes, de transformer ces étapes en méthodes, et de regrouper tous les appels à ces méthodes dans une seule méthode socle, le patron de méthode. Les étapes peuvent être abstraites ou avoir une implémentation par défaut. Pour utiliser l'algorithme, le client doit fournir sa propre sous-classe, implémenter toutes les étapes abstraites, et redéfinir certaines d'entre elles au besoin, mais pas la méthode socle elle-même.

Dans le contexte du logiciel de data mining, une classe de base peut être créée pour les trois algorithmes d'analyse syntaxique (parsing). Cette classe définit une méthode socle composée d'appels à plusieurs étapes traitant les documents. Les étapes abstraites sont déclarées pour forcer les sous-classes à définir leur propre implémentation, ajustant ainsi les signatures des méthodes pour correspondre à celles de la classe mère.

Le code dupliqué peut être éliminé en déplaçant les étapes communes (analyser les données brutes et établir les rapports) dans la classe de base. Cependant, les étapes spécifiques à chaque format (ouvrir/fermer les fichiers, récupérer/parser les données) restent inchangées, préservant la spécificité de chaque sous-classe.

Il existe deux types d'étapes : les opérations abstraites à implémenter dans chaque sous-classe, et les opérations facultatives avec une implémentation par défaut, pouvant être redéfinies si nécessaire. Les crochets (hooks) sont également introduits, des étapes facultatives avec un corps de méthode vide, offrant aux sous-classes des points d'extension supplémentaires avant ou après les étapes cruciales des algorithmes.

![Diagramme UML](https://refactoring.guru/images/patterns/diagrams/iterator/structure-indexed.png)

### Conséquences :

Séparation des Préoccupations : L'utilisation du pattern Iterator sépare la logique de parcours de la structure interne de la collection, favorisant ainsi une meilleure modularité et une gestion des responsabilités plus claire.

Accès Uniforme : Les clients peuvent accéder aux éléments d'une collection de manière uniforme, indépendamment de la structure sous-jacente, ce qui simplifie le code client.

Facilité d'Extension : Ajouter de nouveaux types de collections ne nécessite pas de modifications du code client, pourvu que les collections fournissent un itérateur compatible.

Itérations Concurrentes : Plusieurs itérateurs peuvent parcourir une même collection simultanément et de manière indépendante, offrant une flexibilité d'utilisation.

Réduction de la Complexité : Les algorithmes de parcours sont encapsulés dans les itérateurs, évitant ainsi la complexité de la gestion du parcours directement dans le code client.

### Exemple :

```
from abc import ABC, abstractmethod

//Interface d'Iterateur
class Iterateur(ABC):
    @abstractmethod
    def suivant(self):
        pass

    @abstractmethod
    def a_termine(self):
        pass

    @abstractmethod
    def element_courant(self):
        pass

//Classe Concrète : IterateurListe
class IterateurListe(Iterateur):
    def __init__(self, collection):
        self._collection = collection
        self._position = 0

    def suivant(self):
        self._position += 1

    def a_termine(self):
        return self._position >= len(self._collection)

    def element_courant(self):
        return self._collection[self._position]

//Interface de Collection
class Collection(ABC):
    @abstractmethod
    def creer_iterateur(self):
        pass

//Classe Concrète : Liste
class Liste(Collection):
    def __init__(self):
        self._elements = []

    def creer_iterateur(self):
        return IterateurListe(self._elements)

    def ajouter_element(self, element):
        self._elements.append(element)

ma_liste = Liste()
ma_liste.ajouter_element("Premier")
ma_liste.ajouter_element("Deuxième")
ma_liste.ajouter_element("Troisième")

iterateur = ma_liste.creer_iterateur()

while not iterateur.a_termine():
    print(iterateur.element_courant())
    iterateur.suivant()
```
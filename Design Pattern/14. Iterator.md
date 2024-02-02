## Nom Iterator

### Problématique du Design Pattern Iterator :

Les collections servent principalement de conteneurs pour regrouper des objets et restent parmi les types de données les plus couramment utilisés en programmation.

La plupart des collections stockent leurs éléments dans des listes simples, bien que certaines se basent sur des structures de données plus complexes telles que des piles, des arbres, des graphes, et autres.

Indépendamment de leur structure, une collection doit offrir un moyen d'accéder à ses éléments pour permettre à du code de les utiliser, tout en autorisant le parcours de tous les éléments sans répétition.

Au premier abord, cela semble simple pour les collections ressemblant à des listes ; il suffit de parcourir les éléments dans une boucle. Cependant, comment parcourir séquentiellement les éléments d'une structure complexe telle qu'un arbre ? Un jour, un parcours en profondeur peut suffire, mais le lendemain, un parcours en largeur pourrait être nécessaire. La semaine suivante, une méthode pour un parcours aléatoire pourrait être exigée.

Plus vous ajoutez d'algorithmes de parcours différents à votre collection, plus vous brouillez sa responsabilité principale, qui est de stocker efficacement les données. En outre, certains algorithmes sont spécifiques à un usage particulier, ce qui les rend inappropriés pour être ajoutés au comportement d'une collection générique.

Le code client qui utilise ces collections se désintéresse probablement de la manière dont elles stockent leurs éléments. Cependant, ces collections offrent divers moyens d'accéder à leurs éléments, ce qui inévitablement couple le code client aux classes spécifiques de ces collections.

### Solution :

Le design pattern Iterator vise à extraire le comportement permettant de parcourir une collection et à le placer dans un objet appelé itérateur.

En plus de mettre en œuvre l'algorithme de parcours, un objet itérateur encapsule tous les détails tels que la position actuelle et le nombre d'éléments restants avant d'atteindre la fin. Cela permet à plusieurs itérateurs de parcourir une même collection simultanément et indépendamment les uns des autres.

En général, les itérateurs fournissent une méthode principale pour récupérer les éléments d'une collection. Le client peut appeler cette méthode de manière répétée jusqu'à ce qu'elle ne retourne plus rien, indiquant ainsi que l'itérateur a parcouru tous les éléments.

Tous les itérateurs doivent implémenter la même interface, garantissant ainsi que le code client est compatible avec tous les types de collections et tous les algorithmes de parcours, pourvu que l'itérateur approprié existe. Ainsi, si un parcours particulier est nécessaire dans une collection, il suffit de créer une nouvelle classe d'itérateur sans modifier la collection ni le code client.

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

//Exemple d'Utilisation
ma_liste = Liste()
ma_liste.ajouter_element("Premier")
ma_liste.ajouter_element("Deuxième")
ma_liste.ajouter_element("Troisième")

iterateur = ma_liste.creer_iterateur()

while not iterateur.a_termine():
    print(iterateur.element_courant())
    iterateur.suivant()
```
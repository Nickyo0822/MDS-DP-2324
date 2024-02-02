# Nom : 
Flyweight

# Problématique : 
 Le patron de conception Poids mouche (ou Flyweight en anglais) vise à réduire la consommation de mémoire en partageant efficacement des objets similaires pour minimiser les redondances. Il permet de gérer un grand nombre d'objets en économisant de l'espace mémoire en stockant les parties communes de ces objets de manière centralisée.

# Solution : 
![Diagramme UML](https://dotnettrickscloud.blob.core.windows.net/img/designpatterns/flyweight.png)

La solution consiste à diviser les objets en deux parties distinctes : l'état intrinsèque, qui est partagé et peut être partagé entre plusieurs objets, et l'état extrinsèque, qui est spécifique à chaque objet. Les objets partagent donc leur état intrinsèque, tandis que leur état extrinsèque est passé en paramètre au moment de leur utilisation, permettant ainsi de réduire la consommation de mémoire tout en conservant la flexibilité nécessaire pour les différencier.

```
Le poids mouche n'est pas présent en PHP à cause de la nature du langage.
Un script PHP fonctionne avec une partie des données de l'application et ne charge jamais tout dans la mémoire en même temps.
```

# Conséquences : 
- Avantages : Vous économisez beaucoup de RAM si votre programme est noyé par des tonnes d'objets similaires
- Inconvénients : 
1) Le code devient plus compliqué. Les développeurs qui rejoignent l'équipe vont se demander pourquoi les états d'une entité ont été séparés de cette manière
2) Vous allez gagner en RAM mais perdre en cycles microprocesseur (CPU) si les données du contexte sont recalculées lors de chaque appel d'une méthode poids mouche
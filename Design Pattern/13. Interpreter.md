## Nom Interpreter

### Problématique du Design Pattern Interpréteur :

La problématique abordée par le design pattern Interpréteur concerne la nécessité de définir une grammaire pour un langage particulier et d'interpréter les expressions de ce langage. Il s'agit souvent de transformer des expressions écrites dans ce langage en une représentation interne permettant leur évaluation ou leur exécution.

### Solution :

Le design pattern Interpreter propose une solution en décomposant la grammaire du langage en une hiérarchie de classes d'expressions. Chaque classe représente un élément de la grammaire et offre une méthode pour interpréter cet élément. Les expressions complexes sont construites à partir d'expressions plus simples, formant ainsi une structure arborescente.

Un contexte, souvent représenté par un objet, est utilisé pour maintenir l'état global et les variables nécessaires lors de l'interprétation. Les différentes classes d'expressions coopèrent pour interpréter l'expression complète, en évaluant chaque élément de la grammaire de manière récursive.

### Conséquences :

Flexibilité dans la Définition de la Grammaire : Le pattern Interpréteur permet d'ajouter de nouvelles expressions ou de modifier la grammaire du langage sans altérer les classes existantes.

Complexité de la Grammaire : Pour des langages avec une grammaire complexe, le pattern Interpréteur peut conduire à une multiplication de classes, ce qui peut rendre la gestion de la grammaire elle-même complexe.

Facilité d'Extension : L'ajout de nouvelles expressions ou la modification de la logique d'interprétation peut être réalisé de manière modulaire sans altérer les parties existantes du code.

Structure Arborescente des Expressions : Les expressions sont généralement organisées en une structure arborescente qui peut être parcourue pour évaluer l'ensemble de l'expression.

### Exemple :

```
//Interface d'Expression
class Expression:
    def interpreter(self, context):
        pass

//Expression Terminale : Nombre
class Nombre(Expression):
    def __init__(self, valeur):
        self.valeur = valeur

    def interpreter(self, context):
        return self.valeur

//Expression Non Terminale : Addition
class Addition(Expression):
    def __init__(self, gauche, droite):
        self.gauche = gauche
        self.droite = droite

    def interpreter(self, context):
        return self.gauche.interpreter(context) + self.droite.interpreter(context)

//Expression Non Terminale : Soustraction
class Soustraction(Expression):
    def __init__(self, gauche, droite):
        self.gauche = gauche
        self.droite = droite

    def interpreter(self, context):
        return self.gauche.interpreter(context) - self.droite.interpreter(context)

//Contexte
class Contexte:
    def __init__(self):
        self.variables = {}

expression = Addition(Nombre(5), Soustraction(Nombre(8), Nombre(3)))

contexte = Contexte()
resultat = expression.interpreter(contexte)

print(f"Résultat de l'expression : {resultat}")  # Résultat de l'expression : 10
```
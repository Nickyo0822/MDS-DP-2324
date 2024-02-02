# Nom 
Fabrique

# Description du problème :

Dans de nombreux cas, il est nécessaire de concevoir une classe capable d'instancier divers types d'objets en fonction de paramètres fournis. Par exemple, une usine peut être chargée de fabriquer différents produits en se basant sur le modèle spécifié. La solution la plus évidente serait d'écrire une série de conditions dans cette classe, où, selon le modèle demandé, l'objet correspondant serait instancié et renvoyé.

Cependant, cette implémentation présente un inconvénient majeur. La classe de l'usine devient fortement couplée à tous les types concrets de produits qu'elle peut instancier, car elle dépend directement de leur type. Ce couplage fort devient problématique lorsque le code évolue, par exemple lors de l'ajout de nouveaux produits à fabriquer ou de la suppression de produits obsolètes.

De plus, il est probable que l'instanciation des différents produits soit également effectuée ailleurs dans le code, comme lors de la présentation d'un catalogue des produits fabriqués. Cela peut conduire à une duplication non souhaitée du code, générant ainsi une complexité accrue et une maintenance difficile.

En résumé, la situation engendre un code fortement couplé, potentiellement répliqué en plusieurs endroits de l'application, ce qui compromet la flexibilité, la maintenance, et la gestion des évolutions futures.

# Solution :

Le design pattern Fabrique propose de substituer les appels directs au constructeur d'un objet (utilisant l'opérateur new) en faveur d'une méthode fabrique spéciale. Bien que les objets soient toujours créés à l'aide de l'opérateur new, cet appel est désormais encapsulé à l'intérieur de la méthode fabrique. Les objets ainsi générés sont couramment appelés "produits".

Initialement, cette modification peut sembler anodine, simplement déplaçant l'appel du constructeur dans une autre partie du programme. Cependant, elle offre désormais la possibilité de redéfinir la méthode fabrique dans les sous-classes et de modifier la classe des produits créés par cette méthode.

Une légère limitation subsiste : les sous-classes peuvent retourner des produits différents uniquement si ces produits partagent une classe de base ou une interface commune. En outre, cette interface doit représenter le type retourné par la méthode fabrique de la classe de base.

Par exemple, les classes Camion et Bateau doivent toutes deux implémenter l'interface Transport, qui définit une méthode livrer. Chaque classe implémente cette méthode de manière spécifique : les camions livrent par la route, tandis que les bateaux livrent par la mer. La méthode fabrique de la classe LogistiqueRoute retourne des camions, tandis que celle de la classe LogistiqueMer retourne des bateaux.

Lorsque le code client appelle la méthode fabrique (fréquemment appelé le code client), il ne distingue pas entre les différents produits concrets renvoyés par les sous-classes ; il les considère tous comme des Transports abstraits. Le client sait simplement que tous les objets transportés sont censés posséder une méthode livrer, sans se préoccuper de leurs particularités de fonctionnement.

![Diagramme UML](https://refactoring.guru/images/patterns/diagrams/factory-method/solution1.png?id=fc756d2af296b5b4d482e548214d08ef)

# Exemple (php) :

Le patron Factory peut être utilisé dans des applications de divers langages de programmation. Les exemples les plus connus sont Java, JavaScript, C++, C#, Python et PHP. Ce dernier langage de script est également utilisé dans l'exemple pratique suivant, qui est basé sur un article du blog Phpmonkeys.

Dans ce cas, nous établissons un scénario avec la classe abstraite « Car » (Créateur) et la classe Fabrique « CarFactory » (CréateurConcret). Il est conçu aussi simplement que possible et ne contient qu'un code permettant de définir une couleur pour la voiture, couleur par défaut : blanc, et de la lire :

class Car {
	private $color = null;
	public function __construct() {
		$this->color = "white";
	}
	public function setColor($color) {
		$this->color = $color;
	}
	public function getColor() {
		return $this->color;
	}
}
La classe Fabrique introduit également des méthodes pour les voitures « rouges » (red) et « bleues » (blue), ainsi qu'une méthode privée pour la création de la classe proprement dite :

class CarFactory {
	private function __construct() {
	}
	public static function getBlueCar() {
		return self::getCar("blue");
	}
	public static function getRedCar() {
		return self::getCar("red");
	}
	private static function getCar($color) {
		$car = new Car();
		$car->setColor($color);
		return $car;
	}
}

# Conséquences :

Les avantages de l'utilisation du pattern Décorateur incluent la flexibilité, la réutilisabilité et l'extensibilité. Il permet d'ajouter ou de supprimer des fonctionnalités de manière dynamique, de réutiliser le code existant sans le modifier, et d'ajouter de nouveaux décorateurs sans altérer les classes existantes. Cependant, des inconvénients tels qu'une complexité accrue, une éventuelle redondance, et la sensibilité à l'ordre des décorateurs doivent être pris en compte lors de son utilisation.
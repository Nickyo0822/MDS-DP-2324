### Prototype : Yannick (18/23)

> Le design pattern Prototype permet de créer de nouveaux objets en les clonant à partir de prototypes existants, offrant flexibilité et réduction de la duplication de code.

### Problématique

Le design pattern Prototype répond au problème de la création d'objets complexes en évitant de spécifier leurs classes concrètes. Il est utilisé lorsque la création d'un nouvel objet nécessite une copie d'un objet existant, mais la classe concrète de cet objet n'est pas connue à l'avance.

### Solution

Le design pattern Prototype propose de créer des objets à partir de prototypes (objets existants) en les clonant. Il introduit une interface commune ou une classe abstraite (Prototype) avec une méthode de clonage. Les classes concrètes implémentent cette interface et fournissent leur propre logique de clonage. Ainsi, un client peut créer de nouveaux objets en copiant des prototypes sans connaître leurs classes spécifiques.

### Exemple

```
// Interface Prototype
interface Document {
    public function clone(): Document;
    public function afficher(): void;
}

// Classe Concrète A (DocumentTexte)
class DocumentTexte implements Document {
    private $contenu;

    public function __construct($contenu) {
        $this->contenu = $contenu;
    }

    public function clone(): Document {
        // Clonage de l'objet en utilisant le constructeur de copie
        return new DocumentTexte($this->contenu);
    }

    public function afficher(): void {
        echo "Document Texte : " . $this->contenu . "\n";
    }
}

// Classe Concrète B (DocumentImage)
class DocumentImage implements Document {
    private $cheminImage;

    public function __construct($cheminImage) {
        $this->cheminImage = $cheminImage;
    }

    public function clone(): Document {
        // Clonage de l'objet en utilisant le constructeur de copie
        return new DocumentImage($this->cheminImage);
    }

    public function afficher(): void {
        echo "Document Image : " . $this->cheminImage . "\n";
    }
}

// Client
$prototypeTexte = new DocumentTexte("Contenu initial du texte");
$cloneTexte = $prototypeTexte->clone();

$prototypeImage = new DocumentImage("/chemin/vers/image.jpg");
$cloneImage = $prototypeImage->clone();

// Affichage des documents clonés
$cloneTexte->afficher();
$cloneImage->afficher();
```

### Conséquences

#### Avantages :

- Flexibilité dans la création d'objets : Permet la création d'objets complexes en clonant des prototypes, offrant une alternative à la création directe.
- Réduction de la duplication de code : Évite la duplication de code liée à la création d'objets similaires, car les clients peuvent cloner des prototypes existants.

#### Inconvénients :

- Complexité accrue : L'introduction de prototypes et de clonage peut augmenter la complexité, en particulier lorsque les objets ont des références à d'autres objets.
- Difficulté à cloner des objets complexes : Cloner des objets qui ont des références circulaires ou des liens complexes peut être difficile à gérer.
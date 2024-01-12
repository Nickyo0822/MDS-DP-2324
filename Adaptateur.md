# Nom : 
Adaptateur 

# Problématique : 
Le patron de conception Adaptateur résout le problème d'incompatibilité entre interfaces en permettant à des classes avec des interfaces différentes de collaborer. Il agit comme un intermédiaire qui adapte l'interface d'un composant existant pour la rendre compatible avec le contexte d'utilisation. 

# Solution : 
![Diagramme UML](https://upload.wikimedia.org/wikipedia/commons/3/36/PatternAdapter.png)

Le patron de conception Adaptateur propose une solution en introduisant un adaptateur qui agit comme un intermédiaire entre deux interfaces incompatibles. Il permet à des composants existants avec des interfaces spécifiques de collaborer en convertissant l'interface d'un composant pour la rendre compatible avec celle attendue par le contexte d'utilisation.

```
// Classe Book qui a un atribut title
class Book {
    private $title;

    public function __construct($title) {
        $this->title = $title;
    }

    public function getBookTitle() {
        return $this->title;
    }
}

// Interface attendue par le client
interface TitleInterface {
    public function getTitle();
}

// Adaptateur qui convertit l'interface de Book en celle de TitleInterface
class BookAdapter implements TitleInterface {
    private $book;

    public function __construct(Book $book) {
        $this->book = $book;
    }

    public function getTitle() {
        return "Title: " . $this->book->getBookTitle();
    }
}

// Client utilisant l'interface TitleInterface
function clientCode(TitleInterface $titleInterface) {
    return $titleInterface->getTitle();
}

// Utilisation du code avec l'adaptateur
$book = new Book("Harry Potter");
$bookAdapter = new BookAdapter($book);

echo "Titre du livre : " . clientCode($bookAdapter);
```

# Conséquences : 

- Avantages : 
1) Principe de responsabilité unique. Décuple l'interface ou le code conversion des données, de la logique métier du programme.
2) Principe ouvert/fermé. Possibilité d'ajouter de nouveaux types d'adaptateurs dans le programme sans modifier le code client existant. Ces adaptateurs doivent forcément passer par l'interface du client.
- Inconvénients : La complexité générale du code augmente, car nécessité de créer un ensemble de nouvelles classes et interfaces. Parfois, il est plus simple de modifier la classe du service afin de la faire correspondre avec du code.


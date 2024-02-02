# Nom : 
Observateur 

# Problématique : 
Le patron de conception Observateur (ou Observer en anglais) résout le problème lorsque l'on a plusieurs objets qui doivent réagir automatiquement aux changement d'état d'un autre objet, mais sans qu'ils aient à être fortement couplés. Cela simplifie la gestion des dépendances et permet une communication flexible entre les objets.

# Solution : 
![Diagramme UML](https://upload.wikimedia.org/wikipedia/commons/8/8d/Observer.svg)

Le patron de conception Observateur permet à un objet, appelé sujet, de maintenir une liste d'objets dépendants, les observateurs, qui seront notifiés automatiquement de tout changement d'état du sujet. Ainsi, il offre une solution flexible pour établir une communication sans couplage rigide entre objets, favorisant la réactivité aux changements sans nécessiter une connaissance détaillée des dépendances.

```
// Interface Observateur
interface Observateur {
    public function actualiser(string $nouvelleEdition);
}

// Classe Sujet (Newsletter)
class SujetNewsletter {
    private $observateurs = [];
    private $derniereEdition;

    public function abonner(Observateur $observateur) {
        $this->observateurs[] = $observateur;
    }

    public function desabonner(Observateur $observateur) {
        $index = array_search($observateur, $this->observateurs);
        if ($index !== false) {
            unset($this->observateurs[$index]);
        }
    }

    public function publierNouvelleEdition(string $nouvelleEdition) {
        $this->derniereEdition = $nouvelleEdition;
        $this->notifierObservateurs();
    }

    private function notifierObservateurs() {
        foreach ($this->observateurs as $observateur) {
            $observateur->actualiser($this->derniereEdition);
        }
    }
}

// Classe Observateur (Abonné à la newsletter)
class AbonneNewsletter implements Observateur {
    private $nom;

    public function __construct(string $nom) {
        $this->nom = $nom;
    }

    public function actualiser(string $nouvelleEdition) {
        echo "Bonjour {$this->nom} ! Nouvelle édition de la newsletter : {$nouvelleEdition}\n";
    }
}

// Utilisation
$newsletter = new SujetNewsletter();

$abonne1 = new AbonneNewsletter("Alice");
$abonne2 = new AbonneNewsletter("Bob");

$newsletter->abonner($abonne1);
$newsletter->abonner($abonne2);

$newsletter->publierNouvelleEdition("Numéro 1");

$newsletter->desabonner($abonne1);

$newsletter->publierNouvelleEdition("Numéro 2");

```

# Conséquences : 

- Avantages : 
1) Possibilité d'établir des relations entre les objets lors du lancement de l'application.
2) Principe ouvert/fermé. Possibilité d'ajouter de nouvelles classes souscripteur sans avoir à modifier le code du diffuseur (et inversement si vous avez une interface diffuseur).
- Inconvénients : Les souscripteurs sont avertis dans un ordre aléatoire.


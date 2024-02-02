## Proxy : Yannick (19/23)

> Le design pattern Proxy permet d'introduire un objet intermédiaire qui contrôle l'accès et ajoute des fonctionnalités supplémentaires à un objet réel, tout en fournissant la même interface.

### Problématique

Le design pattern Proxy répond au problème de contrôle d'accès, de gestion de la complexité ou de réduction des coûts associés à l'accès ou à la manipulation d'objets complexes, distants ou coûteux en termes de ressources.

### Solution

![Diagramme UML](https://refactoring.guru/images/patterns/diagrams/proxy/example.png?id=ddb1402479562b9e2c776933cc861bed)

Le design pattern Proxy consiste à introduire un objet de substitution (proxy) qui agit comme une interface intermédiaire pour un objet réel. Ce proxy peut effectuer des tâches supplémentaires, telles que la vérification des autorisations, le chargement paresseux, la mise en cache, etc., avant ou après d'appeler les méthodes de l'objet réel. Il permet de contrôler l'accès à l'objet réel tout en fournissant la même interface que cet objet.

### Exemple

```
// Interface BaseDeDonnees
interface BaseDeDonnees {
    public function lireDonnees(): string;
    public function ecrireDonnees(string $donnees): void;
}

// Classe Concrète BaseDeDonneesConcrete
class BaseDeDonneesConcrete implements BaseDeDonnees {
    private $donnees;

    public function lireDonnees(): string {
        return "Données de la base de données : " . $this->donnees;
    }

    public function ecrireDonnees(string $donnees): void {
        $this->donnees = $donnees;
    }
}

// Classe Proxy ProxyBaseDeDonnees
class ProxyBaseDeDonnees implements BaseDeDonnees {
    private $baseDeDonneesConcrete;
    private $utilisateur;

    public function __construct($utilisateur) {
        $this->utilisateur = $utilisateur;
    }

    public function lireDonnees(): string {
        // Vérification d'autorisation, par exemple, basée sur le nom d'utilisateur
        if ($this->utilisateur === "admin") {
            // Créer la base de données concrète seulement si nécessaire
            if ($this->baseDeDonneesConcrete === null) {
                $this->baseDeDonneesConcrete = new BaseDeDonneesConcrete();
            }

            // Appel à la méthode réelle de la base de données concrète
            return $this->baseDeDonneesConcrete->lireDonnees();
        } else {
            return "Accès refusé. Autorisation insuffisante.";
        }
    }

    public function ecrireDonnees(string $donnees): void {
        // Vérification d'autorisation, par exemple, basée sur le nom d'utilisateur
        if ($this->utilisateur === "admin") {
            // Créer la base de données concrète seulement si nécessaire
            if ($this->baseDeDonneesConcrete === null) {
                $this->baseDeDonneesConcrete = new BaseDeDonneesConcrete();
            }

            // Appel à la méthode réelle de la base de données concrète
            $this->baseDeDonneesConcrete->ecrireDonnees($donnees);
        } else {
            echo "Accès refusé. Autorisation insuffisante.";
        }
    }
}

// Utilisation du Proxy comme objet de base de données
$proxyBaseDeDonnees = new ProxyBaseDeDonnees("admin");

// Lecture des données via le proxy
$donneesLues = $proxyBaseDeDonnees->lireDonnees();
echo $donneesLues . "\n";

// Tentative d'écriture des données via le proxy
$proxyBaseDeDonnees->ecrireDonnees("Nouvelles données.");  // Affichera "Accès refusé. Autorisation insuffisante."
```

### Conséquences

#### Avantages :

- Vous pouvez contrôler l'object service sans que les clients le sachent.
- Vous pouvez gérer le cycle de vie de l'object service lorsque les clients ne s'en soucient pas.
- Le proxy fonctionne même si l'object service n'est pas prêt ou n'est pas disponible.
- Principe ouvert/fermé. Vous pouvez introduire de nouveaux proxys sans modifier le service ou les clients.

#### Inconvénients :

- Le code peut devenir plus compliqué puisque vous devez introduire beaucoup de nouvelles classes.
- La réponse du service peut être retardée.
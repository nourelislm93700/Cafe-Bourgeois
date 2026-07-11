# Café Bourgeois

Site web vitrine et transactionnel pour **Le Bourgeois**, un restaurant gastronomique fictif situé à Paris. Le site permet aux visiteurs de découvrir le restaurant (accueil, menu, brunch), de créer un compte client, de réserver une table et de contacter l'établissement.

Projet réalisé dans le cadre d'un projet universitaire (L3).

## Sommaire

- [Aperçu](#aperçu)
- [Fonctionnalités](#fonctionnalités)
- [Stack technique](#stack-technique)
- [Structure du projet](#structure-du-projet)
- [Base de données](#base-de-données)
- [Installation](#installation)
- [Pages du site](#pages-du-site)
- [Points d'attention et limites connues](#points-dattention-et-limites-connues)
- [Pistes d'amélioration](#pistes-daméllioration)

## Aperçu

Le site est un projet PHP/MySQL classique (architecture LAMP/WAMP), sans framework, structuré autour de pages HTML statiques pour la vitrine et de scripts PHP pour les traitements dynamiques (authentification, réservation, contact, gestion de compte).

## Fonctionnalités

**Côté visiteur**
- Page d'accueil avec diaporama et galerie de plats
- Consultation du menu et de l'offre brunch
- Formulaire de contact (enregistré en base de données)
- Réservation de table sans compte (`reservation.html`)

**Côté client (compte requis)**
- Inscription avec hachage du mot de passe (`password_hash`)
- Connexion avec vérification sécurisée du mot de passe (`password_verify`) et session PHP
- Espace personnel après connexion
- Réservation liée au compte (`client_reservation.html`)
- Historique
- Déconnexion (destruction de session)
- Récupération de mot de passe par e-mail (génération d'un jeton de réinitialisation)

## Stack technique

| Couche | Technologie |
|---|---|
| Frontend | HTML5, CSS3, JavaScript (vanilla) |
| Backend | PHP (extension `mysqli`, requêtes préparées) |
| Base de données | MySQL / MariaDB (dump généré via phpMyAdmin 5.2.1) |
| Serveur | Apache (via WAMP/MAMP/XAMPP) ou équivalent |

Le dépôt contient également un dossier `env1/` correspondant à un environnement virtuel Python (Django, mysqlclient). Ce dossier est vide de tout fichier source une fois décompressé (seule l'arborescence de dossiers a été conservée) et ne fait pas partie du site fonctionnel actuel — voir la section [Points d'attention](#points-dattention-et-limites-connues).

## Structure du projet

```
cafe_bourgeois-main/
├── README.md
└── ProjetL3.zip
    └── ProjetL3/
        ├── Bases_de_données/
        │   ├── cafe_bourgeois.sql       # Dump SQL (structure + données de démo)
        │   └── cafe_bourgeois.png       # Schéma / diagramme de la base
        ├── env1/                        # Environnement virtuel Python (vide, non utilisé)
        └── work/                        # Code source du site
            ├── page_principale.html     # Accueil
            ├── menu.html
            ├── brunch.html
            ├── reservation.html         # Réservation sans compte
            ├── client_reservation.html  # Réservation client connecté
            ├── historique.html
            ├── contact.html
            ├── inscription.html         # Formulaire d'inscription
            ├── espace_1.html            # Formulaire de connexion
            ├── recuperation_de_compte.html
            ├── espace_personnel.php     # Espace client (protégé par session)
            ├── connexion.php
            ├── inscription.php
            ├── reservation.php
            ├── contact.php
            ├── deconnexion.php
            ├── recuperation_mot_de_passe.php
            ├── style1.css
            ├── script_1.js
            └── (images : logo, photos de plats, arrière-plans…)
```

> Le code source est livré dans une archive imbriquée `ProjetL3.zip`. Il est nécessaire de la décompresser pour accéder aux dossiers `Bases_de_données/`, `env1/` et `work/`.

## Base de données

Nom de la base : `cafe_bourgeois`. Le dump `Bases_de_données/cafe_bourgeois.sql` crée quatre tables :

- **`clients`** — `id_client`, `nom`, `prenom`, `email` (unique), `telephone`, `date_naissance`, `mot_de_passe` (haché), `reset_token`
- **`contacts`** — `id`, `nom_complet`, `email`, `sujet`, `message`, `date_creation`
- **`reservations`** — `id_reservation`, `nom_client`, `telephone`, `date_reservation`, `heure`, `nombre_personne`, `message`, `email`
- **`tables_restaurant`** — `id_table`, `numero_table`, `capacite`, `localisation` (plan des tables du restaurant, ex. « Le Face Bar », « Salle Principale »)

Le dump inclut quelques lignes de données de démonstration (un client, une réservation, le plan de salle complet).

## Installation

1. **Prérequis** : un environnement PHP + MySQL, par exemple [WAMP](https://www.wampserver.com/), [MAMP](https://www.mamp.info/) ou [XAMPP](https://www.apachefriends.org/).
2. Décompresser `ProjetL3.zip`, puis copier le contenu du dossier `work/` dans le répertoire servi par Apache (ex. `htdocs/cafe_bourgeois/`).
3. Créer une base de données `cafe_bourgeois` et importer `Bases_de_données/cafe_bourgeois.sql` (via phpMyAdmin ou `mysql -u root -p cafe_bourgeois < cafe_bourgeois.sql`).
4. Mettre à jour les identifiants de connexion à la base dans chacun des fichiers PHP concernés (`connexion.php`, `inscription.php`, `reservation.php`, `contact.php`, `recuperation_mot_de_passe.php`) : variables `$serveur`, `$utilisateur`, `$mot_de_passe`, `$base_de_donnees`. Les identifiants sont actuellement codés en dur et doivent être remplacés par ceux de votre environnement.
5. Pour que la récupération de mot de passe envoie réellement un e-mail, configurer un serveur SMTP local (ou remplacer `mail()` par une librairie comme PHPMailer) et mettre à jour l'URL codée en dur `http://votre-site.com/reset_password.php`.
6. Démarrer Apache/MySQL et ouvrir `http://localhost/cafe_bourgeois/page_principale.html`.

## Pages du site

| Page | Rôle |
|---|---|
| `page_principale.html` | Accueil |
| `menu.html` | Carte du restaurant |
| `brunch.html` | Offre brunch |
| `reservation.html` | Réservation (visiteur non connecté) |
| `client_reservation.html` | Réservation (client connecté) |
| `historique.html` | Historique |
| `contact.html` | Formulaire de contact |
| `inscription.html` | Création de compte |
| `espace_1.html` | Connexion |
| `recuperation_de_compte.html` | Demande de réinitialisation de mot de passe |
| `espace_personnel.php` | Espace client (accès protégé par session) |

## Points d'attention et limites connues

- **Identifiants de base de données en clair** : le mot de passe MySQL est codé en dur dans plusieurs fichiers PHP et versionné dans le dépôt. À corriger avant tout déploiement (variables d'environnement ou fichier de configuration exclu du dépôt).
- **Lien de réinitialisation cassé** : `recuperation_mot_de_passe.php` génère un lien vers `reset_password.php`, fichier absent du projet — la réinitialisation de mot de passe n'est donc pas fonctionnelle de bout en bout.
- **Incohérence de casse** : `espace_personnel.php` référence `Historique.html` (majuscule) alors que le fichier réel est `historique.html` (minuscule) — le lien casse sur un serveur sensible à la casse (Linux).
- **Dossier `env1/` inutile** : environnement virtuel Python/Django présent dans l'archive mais vide une fois décompressé et sans lien avec le site PHP actuel. Il peut être supprimé du dépôt.
- **Absence de protection CSRF** et de limitation de tentatives sur les formulaires (connexion, inscription, contact).
- **Validation** principalement côté client ; peu de validation/assainissement côté serveur au-delà des requêtes préparées.
- Fichier `paris_6` (sans extension, doublon probable de `paris_6.png`) présent dans `work/`.

## Pistes d'amélioration

- Externaliser la configuration de connexion à la base dans un fichier unique (`config.php`) non versionné.
- Implémenter la page `reset_password.php` manquante pour finaliser le parcours de réinitialisation.
- Ajouter des jetons CSRF sur les formulaires POST.
- Remplacer les mots de passe/URL codés en dur par des variables d'environnement.

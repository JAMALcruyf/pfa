
# 🧩 **Système de Gestion des Ressources et des Réservations pour un Espace de Coworking**

## 🎯 **Objectif du projet**

Ce projet consiste à développer un système de gestion des ressources et des réservations pour un espace de coworking. Le système permettra aux clients de :
- Consulter les ressources disponibles (salles, bureaux, matériel),
- Faire une réservation d'une ou plusieurs ressources,
- Effectuer un paiement pour finaliser la réservation.

Les administrateurs auront des privilèges supplémentaires, tels que :
- Ajouter, modifier ou supprimer des ressources,
- Gérer les réservations et les paiements des clients.

## 🔧 **Technologies utilisées**

- **Backend** : Django (Python)
- **Base de données** : SQLite (ou PostgreSQL en production)
- **Frontend** : HTML, CSS, et JavaScript (pour l'interface utilisateur)
- **Authentification** : Django Authentication
- **Gestion des paiements** : Intégration avec une API de paiement (par exemple, Stripe)

---

## 📘 **Diagramme de Classes UML (StarUML)**

Le diagramme de classes décrit la structure de la base de données et les relations entre les différentes entités du système. Ci-dessous le code source à intégrer dans **StarUML** pour générer le diagramme de classes.

Voici le diagramme UML généré pour la structure des classes du projet :

![Diagramme UML de Classes](https://uml.planttext.com/plantuml/png/bLDBJiCm4Dr7oXsiB45PT8iGgbGEW42iApUULYFyasS2gW29Et09kkS6kGadmGvfcwIjHBtol7bFVdvZJubbuDheFCipHjOWRb6kWZG6X09HQKa497u-FiN3chZv-iDdxiM59xIhf6j9uf5H8qc6EeZNF1DnNQ8ILm8jTwLr9jR4eHMsa0_DvWWCHg8UWyRnNC7S9qlZNcY-THzytnkRQGJBEqzn7y07f1FqUo1oTQZWZ5lmsXyuggIN5NAp6OWLK1NGCpex5YWaxoAuS4uF7JKRWl0iMz49Koe67yDPr1pXL30QwFUMQ0yxSVhSAexaS2SHJ3-uqOd6MPOsYPVQoO4FlvN-yy3-qxb-AyjkP7dMZQwO5C95kBWe1c7asI2Qr2xBcvQ-c-AOCzbpjZTC5gPWLVm6lm00)

### 📄 **Code Source UML pour StarUML**

Voici le code source pour générer le diagramme de classes :

```plaintext
@startuml

title Diagramme de classes – Système de Coworking

class Utilisateur {
    +id : int
    +nom : string
    +prénom : string
    +email : string
    +mot_de_passe : string
    +rôle : string
}

class Client {
    +entreprise : string
}

class Ressource {
    +id : int
    +nom : string
    +type : string
    +capacité : int
    +description : string
    +dispo : bool
}

class Réservation {
    +id : int
    +date_debut : datetime
    +date_fin : datetime
    +statut : string
}

class Paiement {
    +id : int
    +montant : float
    +date_paiement : datetime
    +statut : string
}

Utilisateur <|-- Client
Client "1" o-- "*" Réservation
Réservation "*" --> "1" Ressource
Réservation "1" --> "1" Paiement

@enduml
```

### 📝 **Explication du diagramme de classes :**

- **Utilisateur** : Représente l'utilisateur du système. Il possède des informations personnelles comme `nom`, `email` et `mot_de_passe`. Le rôle peut être soit **admin** (administrateur) soit **client**.
  
- **Client** : Hérite de la classe `Utilisateur` et ajoute un attribut supplémentaire, à savoir l'`entreprise` du client. Un client peut avoir plusieurs **réservations**.

- **Ressource** : Représente les différentes ressources disponibles dans l’espace de coworking. Il peut s’agir de salles, de bureaux ou de matériel. Chaque ressource a un `nom`, `type`, `capacité`, `description` et un statut de disponibilité `dispo` (si la ressource est libre ou occupée).

- **Réservation** : Un client peut effectuer plusieurs réservations de ressources. Chaque réservation est associée à une ressource (`ressource_id`), une date de début et de fin (`date_debut`, `date_fin`) et un statut de la réservation (par exemple "en attente", "confirmée", "annulée").

- **Paiement** : Chaque réservation peut être associée à un paiement. Le paiement a un montant, une date de paiement et un statut indiquant si le paiement a été effectué ou non.

---

## 🎭 **Diagramme de Cas d'Utilisation**

Le diagramme de cas d’utilisation présente les interactions principales entre les utilisateurs (acteurs) et le système. Voici une représentation textuelle détaillée de ces interactions.

### 🎯 **Acteurs :**

1. **Client** : Un utilisateur du système qui peut réserver une ressource (bureau, salle, matériel) pour une période donnée.
2. **Administrateur** : Un utilisateur avec des privilèges élevés qui peut gérer les ressources, les réservations, et les paiements.

### 🧩 **Cas d’utilisation :**

#### **Client :**
- **Consulter les ressources disponibles** : Le client peut voir une liste des ressources disponibles (bureaux, salles, matériel), filtrer par type et consulter les détails (capacité, description).
- **Faire une réservation** : Le client peut réserver une ressource pour une période donnée en sélectionnant une ressource et en définissant la durée de la réservation.
- **Modifier / Annuler une réservation** : Si nécessaire, le client peut modifier ou annuler une réservation en fonction de la politique du coworking.
- **Payer la réservation** : Une fois la réservation effectuée, le client peut procéder au paiement via une plateforme de paiement (par exemple, Stripe).

#### **Administrateur :**
- **Ajouter / Modifier / Supprimer des ressources** : L’administrateur peut gérer les ressources du coworking en ajoutant de nouvelles ressources, en modifiant les informations existantes ou en supprimant des ressources obsolètes.
- **Voir les réservations** : L’administrateur peut consulter la liste des réservations effectuées par les clients, avec les détails relatifs à chaque réservation (client, ressource, dates).
- **Gérer les paiements** : L’administrateur peut consulter l’état des paiements pour chaque réservation et intervenir si un paiement a échoué ou est en attente.

---

## 🧑‍💻 **Modèles Django (Backend)**

### Structure des modèles Django correspondant à notre diagramme UML :

```python
from django.db import models
from django.contrib.auth.models import AbstractUser

class Utilisateur(AbstractUser):
    ROLE_CHOICES = [('admin', 'Administrateur'), ('client', 'Client')]
    rôle = models.CharField(max_length=10, choices=ROLE_CHOICES)

class Client(models.Model):
    utilisateur = models.OneToOneField(Utilisateur, on_delete=models.CASCADE)
    entreprise = models.CharField(max_length=100)

class Ressource(models.Model):
    TYPE_CHOICES = [('salle', 'Salle'), ('bureau', 'Bureau'), ('materiel', 'Matériel')]
    nom = models.CharField(max_length=100)
    type = models.CharField(max_length=20, choices=TYPE_CHOICES)
    capacité = models.IntegerField()
    description = models.TextField()
    dispo = models.BooleanField(default=True)

class Réservation(models.Model):
    client = models.ForeignKey(Client, on_delete=models.CASCADE)
    ressource = models.ForeignKey(Ressource, on_delete=models.CASCADE)
    date_debut = models.DateTimeField()
    date_fin = models.DateTimeField()
    statut = models.CharField(max_length=20, default='en attente')

class Paiement(models.Model):
    reservation = models.OneToOneField(Réservation, on_delete=models.CASCADE)
    montant = models.DecimalField(max_digits=8, decimal_places=2)
    date_paiement = models.DateTimeField(auto_now_add=True)
    statut = models.CharField(max_length=20, default='en attente')
```

### **Explication des modèles :**
- **Utilisateur** : Modèle qui hérite de la classe `AbstractUser` de Django pour l’authentification et la gestion des utilisateurs.
- **Client** : Hérite de `Utilisateur`, mais inclut des informations supplémentaires comme l’entreprise du client.
- **Ressource** : Représente les différentes ressources disponibles à la réservation.
- **Réservation** : Associe un client à une ressource pendant une période donnée.
- **Paiement** : Lien entre une réservation et un paiement.

---

## 📑 **Installation et Lancement**

### 1. **Cloner le dépôt :**

```bash
git clone https://github.com/votre-projet/pfa
cd coworking-management
```

### 2. **Créer un environnement virtuel :**

```bash
python -m venv env
source env/bin/activate  # Sur Windows : env\Scripts\activate
```

### 3. **Installer les dépendances :**

```bash
pip install -r requirements.txt
```

### 4. **Appliquer les migrations de la base de données :**

```bash
python manage.py migrate
```

### 5. **Lancer le serveur de développement :**

```bash
python manage.py runserver
```

---

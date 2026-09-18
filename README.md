# 🛡️ Projet de Protocoles de Sécurité Réseau - Groupe 08

## 👥 Membres du Groupe
* **LUWAWA NAMATA GLODY**
* **BEYA KADIMA DIEUDONNE**
* **DELPH KONGOLO**
* **MARDOCHE MUAKA**

---

## 🎯 Objectifs du Projet
Conception, développement et déploiement d'une application web d'éco-domotique sécurisée pour une **Maison Connectée**. L'application démontre concrètement l'atténuation des menaces réseau courantes affectant les environnements IoT via l'implémentation de mécanismes cryptographiques et de politiques de contrôle d'accès strictes.

---

## 🏗️ Architecture et Protocoles Choisis

L'application repose sur trois couches logicielles interconnectées via des canaux de communication chiffrés :

### 1. Protocoles de Sécurité Réseau Implémentés
* **HTTPS / TLS (Transport Layer Security) :** Chiffrement de l'intégralité des flux de requêtes entre les navigateurs clients et le serveur d'API Express, empêchant les attaques d'écoute clandestine (*Eavesdropping*).
* **MQTT over TLS (Port 8883) :** Payload domotique encapsulé et acheminé de manière chiffrée vers un Broker IoT distant en utilisant des certificats d'objets, neutralisant les risques d'interception ou d'injection de fausses commandes physiques sur le réseau domotique.

### 2. Mécanismes Cryptographiques et Applicatifs
* **Authentification & Autorisation (JWT - JSON Web Tokens) :** Utilisation de jetons signés cryptographiquement pour sécuriser les sessions applicatives. Gestion des accès basée sur les rôles (**RBAC**) segmentant strictement les privilèges entre `ADMIN` (Superviseur), `USER` (Résidents), et `GUEST` (Invités).
* **Hachage des Secrets (BcryptJS) :** Mots de passe stockés en base de données sous forme de condensats salés uniques calculés avec un facteur de travail de 10, immunisant le système contre les attaques par dictionnaire ou tables de correspondance.
* **Prévention des Injections SQL :** Utilisation systématique de requêtes préparées avec substitution de paramètres (`?`) via le pilote SQLite3, neutralisant toute tentative de manipulation des tables SQL.

---

## 🛑 Détection comportementale et Pare-feu applicatif (Attaques Traitées)

Pour pallier les faiblesses des objets connectés face aux attaques par saturation de commandes ou Scripts automatisés (DoS), un module de détection d'anomalies en temps réel a été implémenté :

* **Mécanisme :** Algorithme de limitation temporelle (*Rate-Throttling*) analysant l'intervalle entre les requêtes domotiques.
* **Règle :** Si un utilisateur émet plus de 3 requêtes consécutives avec un écart inférieur à 1500 ms, l'action est immédiatement interceptée et avortée.
* **Résultat Réseau :** Renvoi d'un code d'erreur standardisé **HTTP 429 (Too Many Requests)**. L'incident est simultanément enregistré dans la table `security_logs` sous le drapeau d'alerte `ANOMALIE_` et le statut **BLOQUÉ** pour analyse par le superviseur.

---

## ⚠️ Limites du Système (Point 6 des consignes)
Bien que l'infrastructure actuelle sécurise les couches de transport et d'accès, certaines limites subsistent :
1. **Persistance des Sessions :** Les jetons JWT ne disposent pas de liste de révocation automatique serveur (*Blacklist*) avant leur expiration automatique (1h).
2. **Absence de Double Facteur (2FA) :** L'authentification repose uniquement sur le modèle Identifiant/Mot de passe, ce qui expose le système si un utilisateur utilise un mot de passe trop faible ou prédictible.
3. **Stockage Local SQLite :** La base de données SQLite est un fichier local. Pour une infrastructure à grande échelle, une migration vers une base PostgreSQL ou MySQL distante chiffrée serait nécessaire pour assurer la haute disponibilité.

---

## 📦 Citations des Dépendances Externes Utilises
Conformément aux règles d'éthique, voici la liste des modules externes open-source utilisés pour concevoir les briques de sécurité :
* **`express`** : Framework serveur HTTP.
* **`jsonwebtoken`** : Implémentation des standards de jetons d'accès.
* **`bcryptjs`** : Algorithme de hachage et salage de mots de passe.
* **`sqlite3`** : Moteur de base de données embarqué.
* **`mqtt`** : Client de connectivité réseau pour les protocoles IoT chiffrés.
* **`dotenv`** : Gestionnaire de chargement des variables d'environnement isolées.

---

## 🚀 Informations de Démonstration pour l'Évaluation

* **URL Publique du Site (Render) :** [https://onrender.com](https://onrender.com)

### Identifiants de test (Données 100% simulées et fictives)
* **Superviseur (ADMIN) :** `admin` / `AdminPass123!` (À tester sur la page `/admin-gate.html`)
* **Résident (USER) :** `user` / `UserPass123!` (À tester sur la page d'accueil `/index.html`)
* **Invité (GUEST) :** `guest` / `GuestPass123!`



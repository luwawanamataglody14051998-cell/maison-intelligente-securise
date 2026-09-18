
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

## 🚀 Installation et Déploiement

### Déploiement de Démonstration (Production)
* **URL Publique du Site (Render) :** `https://onrender.com` (À remplacer par votre lien Render une fois généré)
* **URL du Dépôt GitHub :** (À remplacer par votre lien de dépôt)

### Identifiants de Démonstration pour l'Évaluation
* **Superviseur (ADMIN) :** `admin` / `AdminPass123!`
* **Résident (USER) :** `user` / `UserPass123!`
* **Invité (GUEST) :** `guest` / `GuestPass123!`

### Exécution en Local
1. Installez les modules requis :
   ```bash
   npm install express jsonwebtoken bcryptjs sqlite3 mqtt dotenv
   ```
2. Créez un fichier `.env` à la racine contenant votre variable `JWT_SECRET`.
3. Démarrez l'instance locale :
   ```bash
   node server.js
   ```

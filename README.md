# ☸️ Compte-Rendu TP Kubernetes - Déploiement MyOpenIA

![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)
![Kubernetes](https://img.shields.io/badge/kubernetes-v1.34.1-blue?style=for-the-badge&logo=kubernetes)
![Docker](https://img.shields.io/badge/Docker-Desktop-2496ED?style=for-the-badge&logo=docker)

Ce projet documente le déploiement initial de la plateforme **MyOpenIA** sur un cluster Kubernetes local. L'objectif est d'orchestrer des micro-services résilients en utilisant une approche déclarative, en isolant les ressources dans un Namespace dédié.

---

## 🏗️ Architecture et Fichiers

L'infrastructure respecte une logique de déploiement par couches. Tous les manifestes se trouvent dans le répertoire `infra/k8s/base/` :

| Fichier | Rôle |
| :--- | :--- |
| `namespace.yaml` | Isolation logique (création de l'espace `myopenia`). |
| `gateway-api` | Point d'entrée de l'application (Deployment + Service). |
| `agent-service` | Service backend secondaire (Deployment + Service). |

---

## 📋 Guide Technique Pas à Pas

### 1️⃣ Vérification et Création de l'environnement
Nous commençons par valider l'état du nœud local et par créer l'espace d'isolation logique.

<details>
<summary>💻 Voir les commandes terminal</summary>

* Vérifier l'état du cluster :
    ```bash
    kubectl get nodes
    ```
* Créer le namespace :
    ```bash
    kubectl apply -f namespace.yaml
    ```
* Vérifier la création :
    ```bash
    kubectl get namespaces
    ```
</details>

![Cluster Prêt](./cluster_prêt.png)
![Namespace Créé](./espace_de_noms_créé.png)

---

### 2️⃣ Déploiement des Services
Application des manifestes pour lancer les Deployments (Pods) et les Services (Réseau).

<details>
<summary>💻 Voir les commandes terminal</summary>

* Appliquer toute la configuration :
    ```bash
    kubectl apply -f .
    ```
* Vérifier que l'infrastructure est opérationnelle :
    ```bash
    kubectl get all -n myopenia
    ```
</details>

![Déploiements Actifs](./déploiements_actifs.png)
> *On observe ici les 2 Pods en statut `Running`, les 2 Services avec leurs IPs internes, et les Deployments prêts (1/1).*

---

### 3️⃣ Accès à l'API (Port-Forwarding)
Les services étant exposés en interne (`ClusterIP`), nous créons un tunnel pour y accéder depuis la machine hôte.

> **🛠️ Difficulté rencontrée & Solution :**
> L'image `nginx:latest` écoute nativement sur le port **80**. Bien que nous ayons initialement pensé au port 8000, nous avons dû adapter la configuration pour mapper le port local **8000** vers le port **80** du conteneur afin que le trafic passe correctement.

<details>
<summary>💻 Voir les commandes terminal</summary>

* Lancer le tunnel (laisser le terminal ouvert) :
    ```bash
    kubectl port-forward svc/gateway-api 8000:80 -n myopenia
    ```
* Tester la réponse de l'API :
    ```bash
    curl http://localhost:8000
    ```
</details>

![Succès Curl](./curl_success.png)

---

### 4️⃣ Test de Résilience (Auto-Guérison)
Démonstration de la capacité de Kubernetes à réparer le système automatiquement (Self-healing).

<details>
<summary>💻 Voir les commandes terminal</summary>

* Supprimer un Pod manuellement pour simuler une panne :
    ```bash
    kubectl delete pod <nom-du-pod> -n myopenia
    ```
* Observer la recréation immédiate :
    ```bash
    kubectl get pods -n myopenia
    ```
</details>

![Auto Guérison](./capture_auto-guérison.png)

**Analyse du résultat :**
Le **Deployment** a détecté que le nombre de répliques actives (0) ne correspondait plus à l'état désiré (1). Il a ordonné la création d'un nouveau Pod immédiatement (Age : 14s sur la capture).

---

## 🧠 Synthèse Technique

### Rôle des ressources
* **Namespace** : Utiliser pour éviter les conflits de nommage avec d'autres projets.
* **Deployment** : "Cerveau" qui gère le cycle de vie des applications et assure la haute disponibilité.
* **Service** : Interface réseau stable (IP fixe) permettant d'accéder aux Pods même s'ils sont redémarrés.

### Ce que Kubernetes gère automatiquement
1.  **L'auto-guérison :** Remplacement automatique des conteneurs défaillants.
2.  **L'état désiré :** Surveillance continue pour aligner la réalité sur les fichiers YAML.

---

## 🚀 Prochaines Étapes
La suite du projet (Séance 2) intégrera :
* 📦 **ConfigMaps** pour externaliser la configuration.
* 🔐 **Secrets** pour gérer les données sensibles.
* 💾 **Volumes** pour rendre les données persistantes.

---
*Projet réalisé par Abdoul Aziz LO - Module Kubernetes*

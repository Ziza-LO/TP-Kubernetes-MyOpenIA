# ☸️ Compte-Rendu TP Kubernetes - Déploiement MyOpenIA

![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)
![Kubernetes](https://img.shields.io/badge/kubernetes-v1.34.1-blue?style=for-the-badge&logo=kubernetes)
![Docker](https://img.shields.io/badge/Docker-Desktop-2496ED?style=for-the-badge&logo=docker)

Ce projet documente la mise en place de l'infrastructure micro-services pour la plateforme **MyOpenIA** sur un cluster Kubernetes local. L'objectif est de déployer une architecture résiliente en utilisant une approche déclarative (manifestes YAML).

## 🏗️ Architecture et Fichiers

L'infrastructure est définie dans le dossier `infra/k8s/base/` et comprend :
* **Namespace** : `myopenia` (Isolation)
* **API Gateway** : Point d'entrée (Nginx)
* **Agent Service** : Service backend (Nginx)

---

## 📋 Guide Technique Pas à Pas

### 1️⃣ Initialisation de l'environnement (Cluster & Namespace)
Vérification de l'état du nœud local et création de l'espace de travail isolé.

<details>
<summary>💻 Voir les commandes terminal</summary>

* Vérifier l'état du cluster :
    ```bash
    kubectl get nodes
    ```
* Créer le namespace :
    ```bash
    kubectl apply -f infra/k8s/base/namespace.yaml
    ```
</details>

![Cluster et Namespace](./espace_de_noms_créé.png)

---

### 2️⃣ Déploiement des Services
Application des manifestes pour lancer les Deployments (Pods) et les Services (Réseau).

<details>
<summary>💻 Voir les commandes terminal</summary>

* Appliquer toute la configuration :
    ```bash
    kubectl apply -f infra/k8s/base/
    ```
* Vérifier que tout est vert (Pods Running, Services créés) :
    ```bash
    kubectl get all -n myopenia
    ```
</details>

![Déploiements Actifs](./déploiements_actifs.png)

---

### 3️⃣ Accès à l'API (Port-Forwarding)
Les services étant internes (`ClusterIP`), nous créons un tunnel pour y accéder depuis la machine hôte.

> **🛠️ Challenge Technique Résolu :**
> L'image Nginx écoute par défaut sur le port **80**. Nous avons configuré le tunnel pour mapper le port local **8000** vers le port **80** du conteneur.

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

![Port Forwarding](./port_forward.png)
![Succès Curl](./curl_success.png)

---

### 4️⃣ Test de Résilience (Auto-Guérison)
Démonstration de la capacité de Kubernetes à réparer le système automatiquement.

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

**Résultat :** Le Deployment a détecté la perte du Pod et en a redémarré un nouveau en moins de 15 secondes (Age: 14s).

---

## 🧠 Concepts Clés
* **Pod** : Unité atomique contenant le conteneur applicatif.
* **Deployment** : Assure le maintien de l'état désiré (Self-healing).
* **Service** : Fournit une IP stable pour accéder aux Pods éphémères.

## 🚀 Prochaines Étapes
La suite du projet (Séance 2) intégrera :
* 📦 **ConfigMaps** pour externaliser la configuration.
* 🔐 **Secrets** pour les données sensibles.
* 💾 **Volumes** pour la persistance des données.

---
*Projet réalisé par Abdoul Aziz LO - Module Kubernetes*

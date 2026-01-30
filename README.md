# ☸️ MyOpenIA - Orchestration Kubernetes (TP1)

![Status](https://img.shields.io/badge/Status-Completed-success)
![K8s Version](https://img.shields.io/badge/Kubernetes-v1.34.1-blue)
![Cluster](https://img.shields.io/badge/Cluster-Docker--Desktop-orange)
![Namespace](https://img.shields.io/badge/Namespace-myopenia-purple)

Ce projet documente le déploiement initial de la plateforme **MyOpenIA**. L'objectif est d'isoler les micro-services applicatifs dans un environnement Kubernetes résilient en utilisant une approche déclarative via des fichiers manifests YAML.

---

## 🏗️ Architecture du Projet

Le déploiement est structuré de manière hiérarchique pour respecter les bonnes pratiques DevOps :
* **Namespace dédié** : `myopenia` pour l'isolation logique des ressources.
* **Arborescence Git** : `infra/k8s/base/` regroupant les fichiers de configuration.
* **Micro-services** : Architecture composée de `gateway-api` et `agent-service`.

---

## 📋 Guide Technique Pas à Pas

### 1️⃣ Vérification de l'Environnement
Validation préalable de la disponibilité du cluster local via Docker Desktop.
```bash
kubectl get nodes

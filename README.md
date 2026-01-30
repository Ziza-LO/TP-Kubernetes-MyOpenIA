# ☸️ MyOpenIA - Orchestration Kubernetes (TP1)

![Status](https://img.shields.io/badge/Status-Completed-success)
![K8s Version](https://img.shields.io/badge/Kubernetes-v1.34.1-blue)
![Cluster](https://img.shields.io/badge/Cluster-Docker--Desktop-orange)
![Namespace](https://img.shields.io/badge/Namespace-myopenia-purple)

[cite_start]Ce projet documente le déploiement initial de la plateforme **MyOpenIA**[cite: 145]. [cite_start]L'objectif est d'isoler les micro-services applicatifs dans un environnement Kubernetes résilient en utilisant une approche déclarative via des fichiers manifests YAML[cite: 151, 157, 373].

---

## 🏗️ Architecture du Projet

[cite_start]Le déploiement est structuré selon la hiérarchie demandée pour le livrable[cite: 328, 338]:
* [cite_start]**Espace de noms dédié** : `myopenia` pour l'isolation logique des ressources[cite: 343, 355].
* [cite_start]**Arborescence Git** : `infra/k8s/base/` regroupe l'ensemble des fichiers de configuration[cite: 332, 339, 342].
* [cite_start]**Micro-services** : Architecture composée de `gateway-api` et `agent-service`[cite: 344, 346].

---

## 📋 Guide Technique Pas à Pas

### 1️⃣ Vérification de l'Environnement
[cite_start]Validation préalable de la disponibilité du cluster local via Docker Desktop[cite: 164, 166].
```bash
kubectl get nodes

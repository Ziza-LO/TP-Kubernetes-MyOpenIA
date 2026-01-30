# ☸️ Compte-Rendu TP Kubernetes - Déploiement MyOpenIA

![Status](https://img.shields.io/badge/Status-Completed-success)
![K8s Version](https://img.shields.io/badge/Kubernetes-v1.34.1-blue)
![User](https://img.shields.io/badge/User-Abdoul_Aziz_LO-orange)

Ce projet documente le déploiement initial de la plateforme **MyOpenIA** sur un cluster Kubernetes. L'objectif est d'utiliser la logique déclarative pour orchestrer des micro-services résilients.

---

## 🏗️ Architecture du Projet

Le déploiement est structuré de manière hiérarchique pour respecter les livrables attendus :
* **Namespace dédié** : `myopenia` pour l'isolation des ressources applicatives.
* **Arborescence Git** : Les manifests sont organisés dans le répertoire `infra/k8s/base/`.
* **Composants** : Déploiement des micro-services `gateway-api` et `agent-service`.

---

## 📋 Guide Technique Pas à Pas

### 1️⃣ Vérification du Cluster
Avant de débuter, nous validons que le node local est opérationnel et en état de marche.
```bash
kubectl get nodes

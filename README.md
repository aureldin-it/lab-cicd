# 🚀 Lab CI/CD : GitHub Actions & Docker

![Build Status](https://img.shields.io/badge/CI%2FCD-GitHub%20Actions-blue?style=for-the-badge&logo=githubactions)
![Docker](https://img.shields.io/badge/Container-Docker-2496ED?style=for-the-badge&logo=docker)
![Debian](https://img.shields.io/badge/OS-Debian-A81D33?style=for-the-badge&logo=debian)

Ce projet est un laboratoire pratique de mise en place d'un pipeline de **Déploiement Continu (CD)** automatisé à l'aide d'un **Runner Auto-hébergé (Self-Hosted)** sous Debian.

---

## 🏗️ Architecture du Pipeline

```text
 [ Développeur ]
        │
    git push
        │
        ▼
 [ GitHub Main ] ────── (Trigger Webhook) ──────┐
                                                │
                                                ▼
 [ VM Debian (srv-deb-01) ] ◄─── (Runner Agent Listening)
        │
        ├─► git pull / checkout
        ├─► docker build
        └─► docker run (Port 8095:80)
```

---

## 🛠️ Composants & Choix Techniques

- **Versionning** : Git avec authentification SSH pour la sécurité.
- **Runner Self-Hosted** : Contourne l'inaccessibilité de l'IP privée locale depuis le cloud GitHub SaaS.
- **Service Systemd** : Le runner s'exécute en arrière-plan et redémarre automatiquement au boot de la VM.
- **Containerisation** : Docker reconstruit et déploie le conteneur Nginx/HTML à chaque modification.

---

## 🚀 Utilisation

### Déclencher un Déploiement

Il suffit de pousser une modification sur la branche `main` :

```bash
git add .
git commit -m "Mise à jour de l'application"
git push origin main
```

### Vérification
L'application est automatiquement déployée et accessible sur le réseau local :
`http://<IP_DEBIAN>:8095`# lab-cicd

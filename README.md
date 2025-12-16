# TPJavaPipeLine-{{ABDELMOUGHIT BERDHI}}

> **Note:** Remplacez `{{ABDELMOUGHIT BERDHI}}` par votre nom et prénom dans le nom du dépôt GitHub.

## 📋 Description du Projet

Ce projet démontre l'utilisation de Jenkins Pipeline avec Docker pour automatiser la construction et les tests d'une application Java Maven.

## 🛠️ Prérequis

Avant de commencer, assurez-vous d'avoir installé:

- **Docker** (version 20.10 ou supérieure)
- **Git**
- **Jenkins** (via Docker)

## 📦 Installation

### 1. Installation de Docker

#### Sur Windows:
- Téléchargez [Docker Desktop pour Windows](https://www.docker.com/products/docker-desktop)
- Installez et redémarrez votre ordinateur


### 2. Construction de l'Image Maven-Git

```bash
# Naviguez vers le dossier du projet
cd TPJavaPipeLine-ABDELMOUGHIT-BERDHI

# Construisez l'image Docker
docker build -t my-maven-git:latest -f Dockerfile.maven .
```

### 3. Installation de Jenkins avec Docker

```bash
# Créez un volume pour Jenkins
docker volume create jenkins_home

# Lancez Jenkins avec accès au Docker socket
docker run -d \
  --name jenkins \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkins/jenkins:lts-jdk11

# Récupérez le mot de passe initial
docker logs jenkins
```

### 4. Configuration de Jenkins

1. Ouvrez votre navigateur: `http://localhost:8080`
2. Entrez le mot de passe initial affiché dans les logs
3. Installez les plugins suggérés
4. Créez un utilisateur admin

### 5. Correction des Permissions Docker

Si vous rencontrez une erreur de permission:

```bash
# Ouvrez un shell dans le container en mode root
docker exec -u root -it jenkins bash

# Dans le container, exécutez:
groupadd docker || true
usermod -aG docker jenkins
chmod 666 /var/run/docker.sock
exit

# Redémarrez Jenkins
docker restart jenkins
```

## 🚀 Utilisation

### Créer le Pipeline dans Jenkins

1. Dans Jenkins, cliquez sur **"New Item"**
2. Nommez votre pipeline: `JavaPipeLine`
3. Sélectionnez **"Pipeline"**
4. Dans la section **Pipeline**, choisissez **"Pipeline script from SCM"**
5. SCM: **Git**
6. Repository URL: `https://github.com/abdelmoughit21/TPJavaPipeLine-ABDELMOUGHIT-BERDHI.git`
7. Script Path: `Jenkinsfile`
8. Sauvegardez

### Lancer le Pipeline

1. Cliquez sur **"Build Now"**
2. Observez l'exécution dans **"Console Output"**


## 📁 Structure du Projet

```
TPJavaPipeLine-{{NomPrénom}}/
├── README.md                 # Ce fichier
├── Jenkinsfile              # Script du pipeline Jenkins
├── Dockerfile.maven         # Image Docker Maven + Git



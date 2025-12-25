# mini-projet-docker : student-list
## Description du projet

Ce projet constitue une preuve de concept (POC) visant à démontrer la conteneurisation d’une application de gestion de liste d’étudiants à l’aide de Docker.
L’application permet d’afficher une liste d’étudiants avec leurs âges et repose sur une architecture découplée composée de deux modules :

 API REST (Flask / Python)  : Fournit les données des étudiants depuis un fichier JSON via des endpoints sécurisés avec authentification HTTP Basic.
Interface Web (PHP / Apache) :Permet à l’utilisateur final de consulter la liste des étudiants en interagissant avec l’API.

Cette approche améliore la portabilité, la fiabilité du déploiement et facilite l’automatisation de l’infrastructure.

## Objectifs du projet

- Améliorer le processus de déploiement de l’application
- Adopter les **bonnes pratiques Docker**
- Mettre en place une **infrastructure en tant que code (IaC)**
- Assurer une infrastructure **modulaire, évolutive et automatisée**
- Déployer et utiliser un **registre Docker privé**

## Structure du projet

.

├── Dockerfile # Image Docker de l’API Flask

├── docker-compose.yml # Orchestration API + Website

├── docker-compose-registry.yml # Registre Docker privé + UI

├── requirements.txt # Dépendances Python

├── simple_api/

│ ├── student_age.py # Code source de l’API

│ └── student_age.json # Données des étudiants

└── website/

└── index.php # Interface utilisateur


## Dockerfile 

###  Étape 1 : Création du Dockerfile (API Flask)

**Actions réalisées :**
- Image de base : python:3.8-buster
- Installation des dépendances système : libsasl2-dev - libldap2-dev- libssl-dev
- Installation des dépendances Python via requirements.txt
- Volume persistant pour les données : /data
- Port exposé : 5000
- Démarrage automatique de l’API Flask
###  Étape 2 : Build de l’image Docker

```bash
docker build -t api:v1.0 . 
```
Vérification de l’image créée : 

```bash
docker images
```

###  Étape 3 : Exécution manuelle du conteneur API

```bash
docker run --name student-list -dp 5000:5000 \
  -v ./student_age.json:/data/student_age.json \
  api:v1.0
```
**Résultat obtenu :**

- L’API Flask s’exécute dans un conteneur Docker

- Les données sont persistantes grâce au volume monté

- L’API est accessible via http://localhost:5000

Vérification du conteneur en cours d’exécution :

```bash
docker ps
```

## Docker Compose – Application
**Service website**
- Image : php:apache
- Port exposé : 80
- Authentification vers l’API via variables d’environnement
- Volume : ./website → /var/www/html
- Démarre après l’API (depends_on)

**Service api**
- Image : api:v1.0
- Port interne : 5000 (non exposé publiquement)
- Volume : student_age.json → /data/student_age.json

**Réseau**

- Type : bridge
- Nom : student-list-networks
- Avantage : communication sécurisée et isolation des services


**Pourquoi utiliser Docker Compose ?**

Docker Compose permet de lancer plusieurs conteneurs ensemble et de gérer :
- Les dépendances
- Les réseaux
- Les volumes
- Les ports
-  Sans docker-compose, il faudrait tout lancer manuellement, ce qui est compliqué et source d’erreurs.
  
**Déploiement du Docker-compose**
```bash
docker compose -f docker-compose.yml up -d
```

cette commande pour vérifier que l'API répond correctement 
```bash
curl -u toto:python -X GET http://<host IP>:<API exposed port>/pozos/api/v1.0/get_student_ages
```

## Registre Docker privé

Un registre Docker privé est déployé afin de stocker et visualiser les images générées.

**Caractéristiques**

- Registre Docker : localhost:5000
- Interface Web : http://localhost:8080
- Authentification :
   - Registre : pozos / pozos
- Fonctionnalités :
  - Suppression des images activée
  - Authentification htpasswd
  - Interface graphique (Joxit UI)
 




**Déploiement du registre**
```bash
docker compose -f docker-compose-registry.yml up -d
```

**Pousser une image vers le registre**
- #### Se connecter au registre
```bash
docker login  -u pozos -p pozos  http://localhost:8080
```

- #### Tagger l'image
```bash
docker tag api:v1.0 localhost:8080/api:v1.0
```
- #### Pousser l'image
```bash
docker push localhost:8080/api:v1.0
```













# mini-projet-docker
Description du projet

Ce projet constitue une preuve de concept (POC) visant à démontrer la conteneurisation d’une application de gestion de liste d’étudiants à l’aide de Docker.

L’application permet d’afficher une liste d’étudiants avec leurs âges et repose sur une architecture découplée composée de deux modules :

API REST (Flask / Python)
Fournit les données des étudiants depuis un fichier JSON via des endpoints sécurisés avec authentification HTTP Basic.

Interface Web (PHP / Apache)
Permet à l’utilisateur final de consulter la liste des étudiants en interagissant avec l’API.

Cette approche améliore la portabilité, la fiabilité du déploiement et facilite l’automatisation de l’infrastructure.

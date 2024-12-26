# Calcul des Racines de Polynômes et Factorisation Symbolique  

## Auteurs  
BENZALA, BOURHAZI, AIT IHSSAN, IBOURKI, BELAHRECH  

## Description  
Ce projet est une solution logicielle modulaire et automatisée pour le calcul des racines de polynômes. Il exploite des microservices pour effectuer des calculs numériques et symboliques à l'aide de bibliothèques comme **Numpy**, **Sympy**, et l'**algorithme de Newton-Raphson**.  

L'application est accessible via des interfaces web et mobiles, permettant aux utilisateurs de soumettre des polynômes, de choisir un algorithme de calcul et de consulter l'historique des résultats.  

## Fonctionnalités principales  
- **Calcul automatique des racines de polynômes** (Numpy, Sympy, Newton-Raphson)  
- **Gestion des utilisateurs** (inscription, connexion, récupération de mot de passe)  
- **Historique des calculs**  
- **Notification par email**  
- **Interfaces Web et Mobile** (ReactJS et Flutter)  
- **Portabilité et Scalabilité** grâce à Docker  

---

## Architecture du logiciel  
L'application est construite autour de quatre microservices principaux :  
- **Microservice de Stockage (Spring Boot)** : Gestion des requêtes utilisateur et stockage  
- **Microservice Numpy (Flask/Python)** : Calcul numérique des racines  
- **Microservice Sympy (Flask/Python)** : Factorisation symbolique et calcul algébrique  
- **Microservice Newton-Raphson (Flask/Python)** : Approximations itératives des racines  

---

## Flux de calcul de racines polynomiales  
Ce diagramme illustre le flux de calcul des racines polynomiales :  

![Diagramme BPMN](./images/BPMN2.jpg)  

---

## Monitoring avec Grafana  
Des tableaux de bord Grafana permettent de surveiller les performances des microservices et d’analyser les indicateurs clés.  

### Taux de succès total  
![Taux de succès total](./images/ratio_success_total.jpg)  

### Ratio de création  
![Ratio de création](./images/ratio_created.jpg)  

### Temps de réponse (Total)  
![Temps de réponse total](./images/reponse_time_sum.jpg)  

### Temps de réponse (Nombre de requêtes)  
![Nombre de requêtes](./images/response_time_count.jpg)  

### Temps de réponse (Global)  
![Temps de réponse global](./images/response_time.jpg)  

---

## Analyse SonarQube  
L'analyse SonarQube garantit la qualité et la sécurité du code source pour chaque microservice.  

### Microservice Newton-Raphson  
![SonarQube Newton-Raphson](./images/microservnewton.jpg)  

### Microservice Numpy  
![SonarQube Numpy](./images/microservnumpy.jpg)  

### Microservice Sympy  
![SonarQube Sympy](./images/microservsympy.jpg)  

### Microservice SPRING BOOT  
![SonarQube SPRING BOOT](./images/microservStockage.jpg)  
---
## Images dans Docker
![Images Docker](./images/imageDockerr2.jpg) 

----
## Docker Compose
```bash
version: '3.8'

networks:
  app-network:
    driver: bridge

services:
  mysql:
    image: mysql:8.0
    container_name: mysqldb
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: polynome
    ports:
      - "3307:3306"
    networks:
      - app-network
    healthcheck:
      test: [ "CMD-SHELL", "mysqladmin ping -h localhost -uroot -proot || exit 1" ]
      interval: 10s
      retries: 10
      start_period: 30s
      timeout: 5s

  spring-app:
    image: polynomespring-app
    container_name: spring-app
    build:
      context: ./MicroServiceStockage_CalculAutomatiseRacinePolynome
      dockerfile: Dockerfile
    ports:
      - "8082:8082"
    networks:
      - app-network
    environment:
      MYSQL_HOST: mysqldb
      MYSQL_USER: root
      MYSQL_PASSWORD: root
      MYSQL_PORT: 3306
    depends_on:
      mysql:
        condition: service_healthy

  python1:
    image: microservicepython1-traitement
    container_name: python1
    build:
      context: ./MicroServiceTraitement-Calcul-Automatis-Racine-Poylynomes
      dockerfile: Dockerfile
    ports:
      - "5110:5110"
    networks:
      - app-network
    environment:
      FLASK_ENV: production

  python2:
    image: microservicepython2-traitement
    container_name: python2
    build:
      context: ./MicroServiceTraitement2-Calcul-Automatis-Racine-Poylynomes
      dockerfile: Dockerfile
    ports:
      - "5001:5001"
    networks:
      - app-network
    environment:
      FLASK_ENV: production

  python3:
    image: microservicepython3-traitement
    container_name: python3
    build:
      context: ./MicroServiceTraitement3-Calcul-Automatis-Racine-Poylynomes
      dockerfile: Dockerfile
    ports:
      - "5004:5004"
    networks:
      - app-network
    environment:
      FLASK_ENV: production

  react-frontend:
    image: polynomial-react-frontend
    container_name: react-frontend
    build:
      context: ./CalculPolynomial_Front_Web
      dockerfile: Dockerfile
    ports:
      - "3000:80"
    networks:
      - app-network
    depends_on:
      - spring-app
```

---
## Utilisation 
### Pour se connecter en tant que calculateur :
email:notaila7@gmail.com
password : password123

### Pour se connecter en tant qu'admin :
email:root@gmail.com
password : root

## Intégration des pipelines

Nous avons intégré un pipeline CI/CD avec Jenkins pour automatiser le processus de développement, de test et de déploiement de l'application. Ce pipeline gère plusieurs étapes essentielles, notamment la gestion des permissions, l'installation des dépendances, le clonage des dépôts, la construction des images Docker pour les différents microservices et le déploiement via Docker Compose. Cette approche garantit un flux de travail efficace, réduit les erreurs manuelles et facilite la mise à jour continue de l'application.

![Images Docker](./images/jenkins.jpg) 
---
## Impact  
Notre projet présente plusieurs impacts significatifs :  
- **Calculs rapides et automatisés** – Réduction du temps de traitement (minutes au lieu d'heures)  
- **Accessibilité** – Application web et mobile accessible en tout lieu  
- **Automatisation** – Moins de tâches répétitives pour les chercheurs et ingénieurs  

---

## Technologies Utilisées  
- **Backend :** Spring Boot, Python
- **Frontend :** ReactJS, Flutter  
- **Containerisation :** Docker  
- **Monitoring :** Grafana, Prometheus  
- **Tests de charge :** Apache JMeter  
- **Analyse de code :** SonarQube  

---


## Installation et Exécution  
### Prérequis :  
- Docker et Docker Compose  
- Node.js et npm (pour React)  
- Flutter SDK  

### Cloner le projet :  
```bash
git clone https://github.com/Bourhazi/ACE_Project
cd ACE_Project
```
---

#### Lancer les Services avec Docker

docker-compose up

---
### Accéder à l'application :
Frontend Web : http://localhost:3000


API Backend : http://localhost:8082

---
### Pour arreter et supprimer  tous les conteneurs en cours d'execution :
docker-compose down

---
### Vidéo démonstration

[Vidéo démonstration](https://streamable.com/1dn6t5)



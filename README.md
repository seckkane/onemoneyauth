# OneMoney – Microservices Architecture

Ce projet contient :
- **Gateway Service**
- **User Service**
- **Keycloak** pour l’authentification, avec version keycloak:26.2.3 (Real me à importer automatiquement avec docker compose)
- **MySQL** pour le stockage des users (synchro avec keycloak) avec version mysql:9.2.0 
- **JHipster Registry** comme Eureka Server pour la découverte de services avec version jhipster-registry:v7.4.0

L’objectif est de pouvoir lancer l’ensemble **sur n’importe quelle machine** via Docker.

---

## 🚀 Lancer le projet

2 prérequis :
- Docker installé
- Docker Compose installé

### 1️⃣ Cloner le projet

```bash
git clone https://github.com/seckkane/onemoneyauth.git
cd onemoneyauth
docker-compose up -d


Requete TEST

Register : POST http://localhost:8080/api/users/register
{
    "username": "m2gl_onemoney",
    "email": "m2gl@exampple.com",
    "prenoms": "M2gl",
    "nom": "onemoney",
    "phoneNumber": "+221776505080",
    "password": "Secure123!"
}

Login : POST http://localhost:8080/api/users/login
{
  "username": "m2gl_onemoney",
  "password": "Secure123!"
}

Acces a eureka : localhost:8761
Lancer en tant que admin cmd pour modifier le host
notepad C:\Windows\System32\drivers\etc\hosts 
ajouter : 127.0.0.1 keycloak

Configuration Minimale pour les clients eureka 
eureka:
  client:
    service-url:
      defaultZone: http://admin:admin@jhipster-registry:8761/eureka/
    registry-fetch-interval-seconds: 5
    register-with-eureka: true
    fetch-registry: true
  instance:
    prefer-ip-address: false
    hostname: user-service ###(a ajuster selon le microservice (account-service, wallet-service, bill-service, recharge-service, transaction-service))
    instance-id: ${spring.application.name}:${spring.application.instance_id:${random.value}}
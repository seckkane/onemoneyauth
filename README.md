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

Keycloak http://localhost:9080 [admin,admin]
Aller Sur le realm jhipster et Realm Settings - User Profile et Creer l'attribut phoneNumber [required]                    # Creation automatique lors de l'import à configurer

Requete TEST - POSTMAN

Register : POST http://localhost:8080/api/users/register              #Remplacez votre email dans le json body
{
  "username": "{{$randomUserName}}",
  "prenoms": "{{$randomFirstName}}",
  "nom": "{{$randomLastName}}",
  "email": "mettezvotreemail@gmail.com",
  "phoneNumber": "+221771234573",
  "pinCode": "1515"
}

Login : POST http://localhost:8080/api/users/login
{
  "phoneNumber": "+221771234573",
  "pinCode": "Secure123!"
}

Profile : GET http://localhost:8080/api/users/profile
//Pas de Body just le token du login en header

Tester change pin et Reintialiser pin , End point dispo dans le contrat d'interface revisé (En haut dans le dossier Doc)

Reset Pin : POST http://localhost:8080/api/users/reset-pin/request
{
    "email": "issaseckkane@gmail.com"
}

Confirm Reset : POST http://localhost:8080/api/users/reset-pin/confirm
{
    "email" : "issaseckkane@gmail.com",
    "otpCode" : "198705",
    "newPinCode" : "1520"
}


Tester Accoount - Voir Read Me Amina

Creer Compte : POST http://localhost:8080/api/accounts
{
  "userId": 1500,
  "accountType": "BANK"
}

Lister Comptes : GET http://localhost:8080/api/accounts/user/150


Tester Cors

try {
console.log('🚀 Envoi de la requête à http://localhost:8080/api/users/login');
console.log('📍 Origin:', window.location.origin); // Port : 19000 / 19001
console.log('📦 Body:', { phoneNumber, pinCode }); // Login

const response = await fetch('http://localhost:8080/api/users/login', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    },
    credentials: 'include', 
    body: JSON.stringify({
        phoneNumber: phoneNumber,
        pinCode: pinCode
    })
});

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


    docker-compose down -v pour supprimer les volumer et tout relancer proprement 
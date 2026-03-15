# README – Alternative IoT Flow (Test via Postman)

## Contexte
La simulation automatique des objets IoT via le service IDS ne fonctionne pas actuellement à cause d’un problème avec l’image Docker Hub.
Pour tester l’authentification et l’envoi des données IoT, vous pouvez utiliser Postman.

## Étape 1 – Récupérer un token OAuth2

**URL :**
```
http://localhost:3005/oauth2/token
```

**Authentication**  
Type : Basic Auth  
```
Username: tutorial-dckr-site-0000-xpresswebapp
Password: tutorial-dckr-site-0000-clientsecret
```

**Headers**
```
Content-Type: application/x-www-form-urlencoded
```

**Body (x-www-form-urlencoded)**
```
(Generated from the frontend)
username: iot_sensor_d7313726-0f20-40ff-96c0-d35bb8baf50f
password: iot_sensor_ce4d09a1-79a0-4c86-bc3a-0948694bd4bb
grant_type: password
```

**Response attendue**  
Vous recevrez un objet JSON contenant :
- access_token
- refresh_token
- expires_in

## Étape 2 – Envoyer des données IoT

**URL :**
```
http://localhost:7897/iot/d
```

**Query Params**
```
(k = api key, i= ID from the frontend)
k: 58900430-14a6-4e98-ba93-f3cdd2e9eafe
i: id2
```

**Headers**
```
X-Auth-Token: <token fourni par Keyrock>
```

**Body**
```
c|100
```
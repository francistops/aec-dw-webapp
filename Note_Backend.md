# Créer l'environment backend d'un serveur web avec api dans docker

## methodologies et terminologies
Notre environnement de développement doit se raprocher le plus possible de notre environment de production pour évité les surprise.

### docker
docker nous permet mieux controller, securiser et reproduire  nos containeur en isolant les processes de notre host, ça nous permet d'avoir le même environment en local que sur le serveur.

### node.js
Node.js est un interpreteur javascript et Express est un server web que nous utilisons pour construire nos route de notre API

### nginx
Le serveur web de front est **NGINX**. il gere les requete web entrante selon ca configuration dans /etc/nginx.conf *à vérifier*
dans notre cas: 80 (http) redirigé sur 443 (https) et redirige api.TON_NOM.ca sur notre container node.js port 3000 en http

**⚙️ Étapes à suivre**

1.  **Déclarer le service NGINX dans docker-compose.yml**\
    → Il s'appelle static-nginx.

2.  **Utiliser HTTPS avec un certificat**

    -   Pour générer le certificat :

        -   generate-cert.sh (Linux/Mac)

        -   generate-cert.ps1 (Windows)

    -   Ces scripts vont créer un dossier avec la commande openssl.

    -   Jusqu'ici, on a donc 3 fichiers :

        -   docker-compose.yml

        -   generate_cert.ps1

        -   generate_cert.sh

3.  **Si on est sur Windows** :

    -   On exécute .\\generate_cert.ps1 dans PowerShell.

    -   Le certificat va se générer, ensuite on tape cd .\\certs\\

    -   On y trouve deux fichiers : server.crt et server.key

    -   Ces fichiers permettent la connexion sécurisée sur notre
        navigateur.

4.  **Ouvrir le port 443 dans docker-compose.yml**

    -   Ligne à ajouter dans la section ports du service nginx :\
        - \"443:443\"

5.  **Créer le fichier nginx.conf**

    -   Fichier de configuration personnalisé pour notre version de
        NGINX.

    -   Il contient notamment une règle pour forcer la redirection vers
        HTTPS.\
        *(voir fichier fourni)*

6.  **Ajouter des volumes dans docker-compose.yml**

    -   On y monte les certificats et la config nginx.\
        *(voir fichier pour les détails)*

7.  **Lancer l'environnement de développement**

    -   Dans PowerShell :\
        docker compose up -d

    -   On peut vérifier avec docker ps

    -   Si jamais il y a un problème :\
        docker logs \[nom_du_container\]

    -   Pour tout arrêter et nettoyer : clear

    -   docker compose down

    -   docker compose up -d

8.  **Créer un dossier api/**

    -   Il va contenir un fichier server.js (fourni)

    -   Ce fichier va créer une route GET à la racine du site

9.  **Toujours dans api/, créer package.json**

    -   Il s'assure que le serveur se démarre bien

    -   Ce fichier est aussi fourni

10. **Créer une image personnalisée**

-   Pour cela, on crée un fichier Dockerfile (fourni aussi)

-   Ensuite, on construit cette image dans docker-compose.yml via
    Node.js

11. **Configurer nginx.conf pour recevoir les requêtes**

-   Le serveur doit écouter sur le port 80

12. **Redémarrer et voir le résultat**

-   docker ps

-   docker compose down

-   docker compose up -d

-   On observe la création de notre image et le comportement du serveur
    NGINX

13. **Tester dans le navigateur**

-   Aller sur https://api.alejandraserna.ca

-   L'interface change et on est bien redirigé en HTTPS

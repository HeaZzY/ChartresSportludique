# Configuration d'un Reverse Proxy avec Nginx sur Debian

Cette documentation décrit comment configurer un reverse proxy sur un serveur Debian avec Nginx pour rediriger le trafic en fonction des noms de domaine.

## Prérequis

- Un serveur Debian avec Nginx installé
- Deux serveurs web internes accessibles depuis le serveur reverse proxy :
  - **Serveur 1** : `192.168.28.40` (ex. `www.chartres.sportludique.fr`)
  - **Serveur 2** : `192.168.28.200` (ex. `ghost.chartres.sportludique.fr`)

## Étapes de Configuration

### 1. Installer Nginx

Si Nginx n'est pas encore installé, installez-le avec la commande suivante :

```bash
sudo apt update
sudo apt install nginx -y
```

### 2. Configurer Nginx en tant que reverse proxy
Sur le serveur proxy, crée un fichier de configuration pour Nginx dans **/etc/nginx/sites-available/reverse_proxy**.

```bash
sudo nano /etc/nginx/sites-available/reverse_proxy
```

```bash
# Reverse proxy pour app1.chartres.sportludique.fr
#server {
#    listen 443 ssl;
#    server_name www.chartres.sportludique.fr;

#    ssl_certificate /etc/ssl/certificat.crt;
#    ssl_certificate_key /etc/ssl/privkey.key;

#    location / {
#        proxy_pass http://www.chartres.sportludique.fr;
#        proxy_set_header Host $host;
#        proxy_set_header X-Real-IP $remote_addr;
#        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
#        proxy_set_header X-Forwarded-Proto $scheme;
#    }
#}

# Reverse proxy pour app2.chartres.sportludique.fr
#server {
#    listen 80;
#    server_name ghost.chartres.sportludique.fr;

#    location / {
#        proxy_pass http://ghost.chartres.sportludique.fr;
#        proxy_set_header Host $host;
#        proxy_set_header X-Real-IP $remote_addr;
#        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
#        proxy_set_header X-Forwarded-Proto $scheme;
#    }
#}

#server {
#    listen 80;
#    server_name www.chartres.sportludique.fr;
#    return 301 https://$host$request_uri;
#}

#server {
#    listen 443 ssl;
#    server_name www.chartres.sportludique.fr;

#    ssl_certificate /etc/ssl/haproxy/certificat.crt;
#    ssl_certificate_key /etc/ssl/haproxy/privkey.key;

    # ... autres directives SSL (comme ssl_protocols, ssl_ciphers, etc.)

#    location / {
#        proxy_pass http://192.168.28.250:8080;
#        include /etc/nginx/proxy_params;
#    }
#}
# Bloc d'équilibrage de charge pour www.chartres.sportludique.fr
# Bloc d'équilibrage de charge pour www.chartres.sportludique.fr
upstream www_chartres_backend {
    # Utilisation de la méthode round-robin pour équilibrer la charge
    # Cela ne gère pas la persistance de session par défaut, mais nous ajouterons un cookie pour suivre la session
    server 192.168.28.40:80;
    server 192.168.28.41:80;
    ip_hash;
}


# Serveur pour le domaine www.chartres.sportludique.fr (port 443)
server {
    listen 443 ssl;
    server_name www.chartres.sportludique.fr;

    ssl_certificate /etc/ssl/haproxy/certificat.crt;
    ssl_certificate_key /etc/ssl/haproxy/privkey.key;

    location / {
        # Utiliser un cookie de session personnalisé pour "stick" à un serveur
        proxy_set_header Cookie $http_cookie;
        proxy_pass http://www_chartres_backend;

        add_header Set-Cookie "sticky_session=$upstream_addr; Path=/; Max-Age=3600";
        include /etc/nginx/proxy_params;
    }
}

server {
    listen 443;
    server_name citation.chartres.sportludique.fr;

    ssl_certificate /etc/ssl/haproxy/citation/certificat2.crt;
    ssl_certificate_key /etc/ssl/haproxy/citation/privatekey.key;

    location / {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_pass http://citation_chartres_backend;


        include /etc/nginx/proxy_params;
    }
}

upstream citation_chartres_backend {
    # Utilisation de la méthode round-robin pour équilibrer la charge
    # Cela ne gère pas la persistance de session par défaut, mais nous ajouterons un cookie pour suivre la session
    server 192.168.28.200:80;
}
```

Cette configuration fait en sorte que :

    - Les requêtes vers app1.chartres.sportludique.fr sont redirigées vers 192.168.28.30.
    - Les requêtes vers app2.chartres.sportludique.fr sont redirigées vers 192.168.28.200.

### 3. Activer la configuration du reverse proxy
Crée un lien symbolique pour activer cette configuration dans Nginx :

```bash
sudo ln -s /etc/nginx/sites-available/reverse_proxy /etc/nginx/sites-enabled/
```

### 4. Tester la configuration Nginx
Avant de redémarrer Nginx, vérifie que la configuration est correcte :

```bash
sudo nginx -t
```

### 5. Redémarrer Nginx
Redémarre Nginx pour appliquer les changements :

```bash
sudo systemctl restart nginx
```
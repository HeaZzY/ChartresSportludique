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
# Reverse proxy pour www.chartres.sportludique.fr
server {
    listen 80;
    server_name www.chartres.sportludique.fr;

    location / {
        proxy_pass http://192.168.28.40;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

# Reverse proxy pour ghost.chartres.sportludique.fr
server {
    listen 80;
    server_name ghost.chartres.sportludique.fr;

    location / {
        proxy_pass http://192.168.28.200;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
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
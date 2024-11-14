# Configuration du serveur NTP sur Debian

## Introduction
Cette page explique comment installer et configurer un serveur NTP sur un système Debian, ainsi que le processus de désinstallation si nécessaire.

## Installation de NTP
Pour installer le serveur NTP, mettez à jour votre système et installez le paquet NTP. Cela permet à votre serveur de se synchroniser avec des serveurs de temps fiables.

```bash
sudo apt update
sudo apt install ntp
```

## Configuration de `/etc/ntp.conf`
Le fichier de configuration principal de NTP est `/etc/ntp.conf`. Il est recommandé de le modifier pour spécifier des serveurs NTP en France et ajuster les restrictions d'accès afin de sécuriser votre serveur.

```bash
sudo nano /etc/ntp.conf
```

### Exemple de configuration

```bash
# Utilisation de pools NTP en France
pool 0.fr.pool.ntp.org iburst
pool 1.fr.pool.ntp.org iburst
pool 2.fr.pool.ntp.org iburst
pool 3.fr.pool.ntp.org iburst

# Options de restriction pour sécuriser le serveur
restrict default kod nomodify notrap nopeer noquery
restrict 127.0.0.1
restrict ::1

# Exemple de restriction pour un réseau local (facultatif)
# restrict 192.168.0.0 mask 255.255.255.0 nomodify notrap
```

Remarque : L'option iburst permet une synchronisation plus rapide lors du démarrage.

### Points importants à configurer :
- Ajouter des serveurs NTP publics situés en France pour une meilleure précision.
- Appliquer des restrictions de sécurité pour limiter l'accès non autorisé.
- Configurer les options pour un réseau local si nécessaire.

## Définir le fuseau horaire
Assurez-vous que le système utilise le fuseau horaire de Paris pour afficher l'heure correcte. Cela garantit que les journaux et l'heure système sont en phase avec l'heure locale.

## Redémarrage du service NTP
Après la modification de la configuration, redémarrez le service pour appliquer les changements. Cela permet au service de se resynchroniser avec les serveurs NTP définis.

## Vérification du statut de NTP
Pour vérifier le bon fonctionnement de NTP, consultez le statut du service et la liste des serveurs NTP connectés. Cela assure que la synchronisation s'effectue correctement.

## Désinstallation de NTP
Si vous devez supprimer NTP, arrêtez d'abord le service, désinstallez le paquet et supprimez éventuellement les fichiers de configuration restants. Nettoyez les dépendances inutilisées pour maintenir le système optimisé.

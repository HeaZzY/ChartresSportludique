# Unbound Resolver LAN Configuration

## Introduction

Ce document décrit la configuration du résolveur DNS `Unbound` sur Debian pour un réseau LAN. `Unbound` est un serveur DNS récursif, rapide et sécurisé. Il a été configuré pour rediriger les requêtes DNS vers des serveurs spécifiques pour certains domaines, tout en permettant une gestion centralisée des résolutions DNS locales.

## Prérequis

- Un serveur Debian avec `Unbound` installé.
- Accès administrateur (root ou sudo).

### Installation de Unbound

Si `Unbound` n'est pas encore installé, vous pouvez l'installer avec la commande suivante :

```bash
sudo apt update
sudo apt install unbound
```

## Fichier de configuration Unbound
Le fichier principal de configuration se trouve généralement à l'emplacement suivant :

```bash
/etc/unbound/unbound.conf
```

La configuration de votre serveur est la suivante :

```conf
# Fichier de configuration Unbound pour Debian
#
# Voir la page de man unbound.conf(5)
#
# Voir /usr/share/doc/unbound/examples/unbound.conf pour un fichier de configuration de référence commenté.
#
# La ligne suivante inclut des fichiers de configuration supplémentaires dans le répertoire /etc/unbound/unbound.conf.d
include-toplevel: "/etc/unbound/unbound.conf.d/*.conf"

server:
    # Adresse du serveur de résolveur par défaut
    do-not-query-localhost: no
    interface: 0.0.0.0
    access-control: 0.0.0.0/0 allow
    verbosity: 1

forward-zone:
    name: "."
    forward-addr: 121.183.90.205

# Redirection des requêtes pour chartres.sportludique.fr
forward-zone:
    name: "chartres.sportludique.fr."
    forward-addr: 192.168.28.110
```

## Explication de la configuration
- server:

    - do-not-query-localhost: no: Le serveur peut interroger localhost pour résoudre des noms.
    - interface: 0.0.0.0: Le serveur écoute sur toutes les interfaces réseau.
    - access-control: 0.0.0.0/0 allow: Autorise l'accès de toutes les adresses IP.
    - verbosity: 1: Définit le niveau de verbosité des logs.
- forward-zone:

    - name: ".": Ce bloc définit la zone racine (".") pour les requêtes DNS globales.

    - forward-addr: 121.183.90.205: Redirige toutes les requêtes vers l'adresse IP spécifiée pour la résolution.

    - name: "chartres.sportludique.fr.": Redirige les requêtes vers le domaine chartres.sportludique.fr.

    - forward-addr: 192.168.28.110: Serveur DNS spécifique pour ce domaine.

# Commandes Unbound de base
1. Vérification de la configuration :

Avant de redémarrer le service Unbound, vous pouvez vérifier que la configuration est correcte avec :
```bash
sudo unbound-checkconf
```

2. Redémarrer Unbound :

Pour appliquer les changements après modification de la configuration :
```bash
sudo systemctl restart unbound
```
3. Vérifier l'état d'Unbound :

Pour vérifier si le service Unbound fonctionne correctement :
```bash
sudo systemctl status unbound
```

4. Logs Unbound :

Pour consulter les logs d'Unbound, utilisez la commande suivante :
```bash
sudo journalctl -u unbound
```

## Conclusion
Avec cette configuration, le serveur DNS Unbound écoute sur toutes les interfaces réseau et redirige les requêtes globales vers le serveur DNS à l'adresse 121.183.90.205. Les requêtes spécifiques au domaine chartres.sportludique.fr sont redirigées vers le serveur DNS à l'adresse 192.168.28.110.


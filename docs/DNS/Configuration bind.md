# BIND DNS Configuration on DMZ (Public)

## Introduction

Ce document décrit la configuration de `BIND` sur un serveur DNS hébergé dans une DMZ publique. Le serveur DNS utilise des vues pour différencier les requêtes provenant du réseau interne, de la DMZ publique et privée, ainsi que des requêtes externes. Deux zones DNS distinctes (interne et externe) sont configurées pour répondre de manière appropriée selon la provenance des requêtes.

## Prérequis

- Un serveur Debian (ou similaire) avec `BIND` installé.
- Accès administrateur (root ou sudo) pour modifier les fichiers de configuration.
- La configuration est basée sur des réseaux LAN et DMZ, et sur une zone publique accessible de l'extérieur.

### Installation de BIND

Si `BIND` n'est pas encore installé, vous pouvez l'installer avec la commande suivante :

```bash
sudo apt update
sudo apt install bind9 bind9utils bind9-doc
```

## Fichier de configuration : /etc/bind/named.conf.local
Le fichier suivant configure les vues DNS (zones internes et externes) pour gérer les requêtes selon leur provenance.


```conf
view "inside" {

    match-clients {
        172.28.96.0/19;    // Requêtes provenant du LAN
        192.168.28.0/24;   // Requêtes provenant de la DMZ PUB
        192.168.128.0/24;  // Requêtes provenant de la DMZ PRIV
    };

    zone "chartres.sportludique.fr." {
        type master;
        file "/etc/bind/db.interne";
    };
};

// Zone externe
view "outside" {
    match-clients { 
        !172.28.96.0/19;    // Requêtes ne provenant pas du LAN
        !192.168.28.0/24;   // Requêtes ne provenant pas de la DMZ PUB
        !192.168.128.0/24;  // Requêtes ne provenant pas de la DMZ PRIV
        any;                // Toutes les autres adresses
    };

    zone "chartres.sportludique.fr." {
        type master;
        file "/etc/bind/db.externe";
    };
};
```


## Explication de la configuration
- Vues DNS :
- BIND utilise des vues pour répondre différemment aux requêtes en fonction de la source des clients.

    - Vue "inside" : Cette vue s'applique aux clients internes (LAN et DMZ). Elle utilise la zone DNS interne (db.interne) pour répondre avec des adresses IP privées.

    - Vue "outside" : Cette vue est appliquée à toutes les autres requêtes (clients externes). Elle utilise la zone externe (db.externe) pour répondre avec des adresses IP publiques.

- Zones DNS :
- Les deux zones configurent la résolution des noms pour le domaine chartres.sportludique.fr.

    - La zone interne fournit des adresses IP locales pour les services internes.
    - La zone externe fournit des adresses IP publiques pour les services accessibles depuis l'extérieur.

## Configuration des fichiers de zone
Zone externe : **/etc/bind/db.externe**
Ce fichier contient les enregistrements DNS publics pour le domaine chartres.sportludique.fr. Il est accessible aux clients externes via la vue "outside".

```conf
;
; BIND data file for chart (public)
;
$TTL    604800
@       IN      SOA     ns1.chartres.sportludique.fr. admin.chartres.sportludique.fr. (
                         1         ; Serial
                    604800         ; Refresh
                     86400         ; Retry
                   2419200         ; Expire
                    604800 )       ; Negative Cache TTL
;
@       IN      NS      ns1.chartres.sportludique.fr.
ns1     IN      A       183.44.28.1      ; Adresse IP publique pour ns1
www     IN      A       183.44.28.1      ; Adresse IP pour www.chartres.sportludique.fr
```

## Zone interne : /etc/bind/db.interne
Ce fichier contient les enregistrements DNS internes pour le domaine chartres.sportludique.fr. Il est utilisé pour les requêtes provenant du LAN et des DMZs (publique et privée).

```conf
;
; BIND data file for chartres.sportludique.fr (private)
;
$TTL    604800
@       IN      SOA     ns1.chartres.sportludique.fr. admin.lan.sportludique.fr. (
                         1         ; Serial
                    604800         ; Refresh
                     86400         ; Retry
                   2419200         ; Expire
                    604800 )       ; Negative Cache TTL
;
@       IN      NS      ns1.chartres.sportludique.fr.
ns1     IN      A       192.168.28.110      ; Adresse IP privée pour ns1
www     IN      A       192.168.28.40      ; Adresse IP privée pour www.chartres.sportludique.fr

; Délégation du DNS
lan	IN	NS	dc01.lan.chartres.sportludique.fr.
dc01.lan	IN	A	172.28.127.10  ; Adresse du DC pour le sous-domaine lan.chartres.sportludique.fr
```

## Explication des enregistrements DNS
Enregistrement SOA :

Déclare l'autorité de la zone avec le serveur DNS principal ns1.chartres.sportludique.fr.
L'email administrateur est admin.chartres.sportludique.fr (externe) ou admin.lan.sportludique.fr (interne).
Enregistrement NS :

Définit ns1.chartres.sportludique.fr comme serveur de noms.
Enregistrements A :

Associe les noms d'hôte (ex. www, ns1) avec des adresses IP. L'IP dépend de la vue : publique (183.44.28.1) pour la vue externe, et privée (192.168.28.110) pour la vue interne.
Délégation DNS (interne) :

Le sous-domaine lan.chartres.sportludique.fr est délégué à un autre serveur DNS (dc01.lan.chartres.sportludique.fr.) pour des résolutions internes.


1. Verification de la configuration
```bash
sudo named-checkconf
```

2. Redémarage des services

```bash
sudo systemctl restart bind9
```
## Conclusion
Cette configuration de BIND permet de séparer les requêtes DNS en fonction de leur origine (interne ou externe). Les clients internes reçoivent des réponses avec des adresses IP locales tandis que les clients externes reçoivent des adresses IP publiques, garantissant ainsi une gestion flexible et sécurisée des résolutions DNS dans un environnement multi-zones.
# Configuration d'un client NTP pour se synchroniser avec un serveur NTP sous Debian

## Introduction
Cette page détaille la configuration d'un système Linux pour se synchroniser avec un serveur NTP interne.

## Prérequis
Assurez-vous que le serveur NTP est opérationnel et que le port 123/UDP est ouvert pour permettre la communication entre le client et le serveur.

## Installation du client NTP
Installez le paquet `ntp` pour permettre la synchronisation de l'heure.

```bash
sudo apt install ntp
```

## Modification de la configuration
Pour synchroniser un client avec votre serveur NTP, modifiez le fichier de configuration approprié.

### Pour `ntpd`
- **Fichier de configuration** : `/etc/ntpsec/ntp.conf`
- **Modifications** : Ajoutez l'adresse IP ou le nom de domaine de votre serveur NTP dans la configuration du client.
  - Exemple : `server ntp.chartres.sportludique.fr OU 192.168.1.100 iburst`

## Redémarrage du service
Après avoir modifié la configuration, redémarrez le service pour que les modifications prennent effet.

## Vérification de la synchronisation
Assurez-vous que le client se synchronise correctement en vérifiant le statut avec les commandes appropriées :
- Pour `ntpd` : `ntpq -p` ou `date`

## Conclusion
En suivant cette configuration, votre client Linux se synchronisera avec le serveur NTP interne que vous avez configuré.

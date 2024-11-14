---
title: "Configuration des routeurs"
---

# Informations générales du routeur 1 (CHA_RT1)

| **Paramètre**              | **Valeur**                                          |
|----------------------------|----------------------------------------------------|
| **Version**                 | 15.1                                               |
| **Nom de domaine**          | `sportludique.fr`                                  |
| **Hostname**                | CHA_RT1                                            |
| **Utilisateur 1**           | `mathys` (privilege 15, mot de passe chiffré)      |
| **Utilisateur 2**           | `seb` (privilege 15, mot de passe chiffré)         |
| **IP LAN principale**       | 192.168.248.252 (GigabitEthernet0/0.248)           |
| **IP LAN secondaire**       | 10.10.240.3 (GigabitEthernet0/0.240)               |
| **IP WAN**                  | 183.44.28.1 (GigabitEthernet0/1)                   |
| **VIP (haute dispo)**       | 192.168.248.254 (partagé avec CHA_RT2)             |

# Interfaces du routeur 1 (CHA_RT1)

| **Interface**               | **IP**                           | **Description**                    |
|-----------------------------|----------------------------------|------------------------------------|
| Embedded-Service-Engine0/0   | Aucune                          | Désactivée                         |
| GigabitEthernet0/0.240       | 10.10.240.3 /24                 | VLAN 240                           |
| GigabitEthernet0/0.248       | 192.168.248.252 /24             | LAN principale, avec VIP           |
| GigabitEthernet0/1           | 183.44.28.1 /30                 | WAN                                |
| Serial0/1/0                  | Aucune                          | Désactivée                         |

# Configuration NAT du routeur 1 (CHA_RT1)

| **NAT Source**              | **Interface**                    | **Description**                    |
|-----------------------------|----------------------------------|------------------------------------|
| Access-list 1               | GigabitEthernet0/1               | NAT overload pour 192.168.248.0/24 |
| Access-list 2               | GigabitEthernet0/1               | NAT overload pour 172.28.96.0/19   |
| Access-list 3               | GigabitEthernet0/1               | NAT overload pour 192.168.43.0/24  |
| Access-list 4               | GigabitEthernet0/1               | NAT overload pour 192.168.28.0/24  |
| Access-list 5               | GigabitEthernet0/1               | NAT overload pour 192.168.128.0/24 |

# Routes statiques du routeur 1 (CHA_RT1)

| **Destination**             | **Next-hop**                     | **Description**                    |
|-----------------------------|----------------------------------|------------------------------------|
| 0.0.0.0/0                   | 183.44.28.2                      | Route par défaut                   |
| 172.28.96.0/19              | 192.168.248.10                   | Route pour le VLAN 245             |
| 192.168.28.0/24             | 192.168.248.10                   | Route spécifique                   |
| 192.168.43.0/24             | 192.168.248.10                   | Route spécifique                   |
| 192.168.128.0/24            | 192.168.248.10                   | Route spécifique                   |

# Informations du routeur 2 (CHA_RT2)

| **Paramètre**               | **Valeur**                                          |
|-----------------------------|----------------------------------------------------|
| **Hostname**                | CHA_RT2                                            |
| **IP LAN principale**       | 192.168.248.253 (GigabitEthernet0/0.248)           |
| **VIP (haute dispo)**       | 192.168.248.254 (partagé avec CHA_RT1)             |
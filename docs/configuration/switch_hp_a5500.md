---
title: "Configuration switch hp A5500"
---

# Configuration du Cœur de Réseau - HP-CORE

## Informations Système

| Paramètre               | Valeur                         |
|-------------------------|--------------------------------|
| **Nom du Système**       | HP-CORE                        |
| **Version**              | 5.20.99, Release 2222P08       |
| **DHCP Relay Server**    | 172.28.127.10                  |
| **Telnet**               | Activé                         |
| **HTTP**                 | Désactivé                      |
| **SSH**                  | Activé                         |
| **Password Recovery**    | Activé                         |
| **IRF**                  | Activé, membre 1 priorité 32   |
| **Route par Défaut**     | 0.0.0.0/0 via 192.168.43.254   |

---

## VLANs

| VLAN ID  | Nom              | Adresse IP               | Masque de sous-réseau | Relay DHCP |
|----------|------------------|--------------------------|-----------------------|------------|
| **1**    | (par défaut)      | N/A                      | N/A                   | N/A        |
| **240**  | ADMINISTRATION    | 172.28.127.1             | 255.255.255.0         | N/A        |
| **241**  | Direction-G       | 172.28.96.1              | 255.255.255.0         | Oui        |
| **242**  | DSI               | N/A                      | N/A                   | Oui        |
| **243**  | INTERCO 1         | 192.168.43.1             | 255.255.255.0         | N/A        |
| **244**  | INTERCO 2         | N/A                      | N/A                   | N/A        |
| **248**  | WAN               | N/A                      | N/A                   | N/A        |

---

## Interfaces Physiques **up**

| Interface                     | Type        | Mode        | VLAN(s)                | Adresse IP             | Masque de sous-réseau   |
|-------------------------------|-------------|-------------|------------------------|------------------------|-------------------------|
| **GigabitEthernet1/0/1**       | Physique    | Access      | 240                    | N/A                    | N/A                     |
| **GigabitEthernet1/0/2**       | Physique    | Access      | 240                    | N/A                    | N/A                     |
| **GigabitEthernet1/0/3**       | Physique    | Access      | 240                    | N/A                    | N/A                     |
| **GigabitEthernet1/0/4**       | Physique    | Trunk       | 1, 240-249             | N/A                    | N/A                     |
| **GigabitEthernet1/0/6**       | Physique    | Trunk       | 1, 240, 243-244        | N/A                    | N/A                     |
| **GigabitEthernet2/0/1**       | Physique    | Trunk       | 1, 240, 248            | N/A                    | N/A                     |
| **GigabitEthernet2/0/2**       | Physique    | Trunk       | 1, 240, 248            | N/A                    | N/A                     |
| **GigabitEthernet2/0/3**       | Physique    | Trunk       | 1, 240-249             | N/A                    | N/A                     |
| **GigabitEthernet2/0/8**       | Physique    | Access      | 248                    | N/A                    | N/A                     |

---

## Routage Statique

| Destination   | Masque           | Passerelle          |
|---------------|------------------|---------------------|
| **0.0.0.0**   | 0.0.0.0          | 192.168.43.254      |

---

## Utilisateurs Locaux

| Nom d'utilisateur | Type d'accès            | Niveau d'autorisation |
|-------------------|-------------------------|-----------------------|
| **admin**         | SSH, Telnet              | 3                     |
| **da**            | SSH                      | 3                     |
| **mathys**        | SSH                      | 3                     |
| **seb**           | SSH                      | 3                     |

---

## Groupes d'Utilisateurs

| Groupe            | Attributs                |
|-------------------|--------------------------|
| **system**        | allow-guest               |

---

## Configuration IRF

| Membres      | Priorité |
|--------------|----------|
| **Membre 1** | 32       |
| **Membre 2** | 31       |

| IRF Port     | Interface associée            |
|--------------|-------------------------------|
| **1/1**      | Ten-GigabitEthernet1/1/1      |
| **1/2**      | Ten-GigabitEthernet1/1/2      |
| **2/1**      | Ten-GigabitEthernet2/1/1      |
| **2/2**      | Ten-GigabitEthernet2/1/2      |

---

## Paramètres Divers

| Service             | État        |
|---------------------|-------------|
| **Telnet**           | Activé      |
| **SSH**              | Activé      |
| **DHCP**             | Activé      |
| **CWMP**             | Désactivé   |
| **HTTP**             | Désactivé   |
| **Password Recovery**| Activé      |



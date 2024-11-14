---
title: "Stack switch hp A5500"
---
 
# Tutoriel : Configuration de Stack sur Switches HP A5500
 
## Contexte:
Nous avons 2 switch HP A5500 1 configuré sur l'ip 192.168.1.1 temporairement et 1 non configuré.
Nous aimerions les stacké afin que ces 2 switch ne forment qu'un. Nous avons donc raccordé 2 ports LSPX comme suit:

![Raccordement](images/switch.jpg)
 
## Switch 1 : Configuration Initiale
 
### 1. Accéder au Mode de Configuration
 
```plaintext
<HP> system-view
```
 
### 2. Définir la Priorité de l'IRF
```plaintext
<HP> system-view
[HP] irf member 1 priority 32
```
 
 
### 5.Désactiver les Interfaces 10-Gigabit Ethernet
 
```plaintext
[HP] interface ten 1/1/1
[HP-Interface-Ten-GigabitEthernet1/1/1] shut
[HP-Interface-Ten-GigabitEthernet1/1/1] quit
[HP] interface ten 1/1/2
[HP-Interface-Ten-GigabitEthernet1/1/2] shut
[HP-Interface-Ten-GigabitEthernet1/1/2] quit
```
 
### 6. Configurer les Ports IRF
 
```plaintext
[HP] irf-port 1/1
[HP-irf-port1/1] port group interface Ten-GigabitEthernet 1/1/1
[HP-irf-port1/1] quit
[HP] irf-port 1/2
[HP-irf-port1/2] port group interface Ten-GigabitEthernet 1/1/2
[HP-irf-port1/2] quit
```
 
### 7. Réactiver les Interfaces
 
```plaintext
[HP] interface ten 1/1/1
[HP-Interface-Ten-GigabitEthernet1/1/1] undo shut
[HP-Interface-Ten-GigabitEthernet1/1/1] quit
[HP] interface ten 1/1/2
[HP-Interface-Ten-GigabitEthernet1/1/2] undo shut
[HP-Interface-Ten-GigabitEthernet1/1/2] quit
```
 
### 8. Activer la Configuration IRF
 
```plaintext
[HP] irf-port-configuration active
```
 
### 9. Sauvegarder et Rebooter
 
```plaintext
[HP] save
<HP> reboot
```
 
## Switch 2 : Configuration Initiale
 
### 1. Accéder au Mode de Configuration
 
```plaintext
<HP> system-view
```
### 2. Changer l'ID du Switch
```plaintext
[HP] irf member 1 renumber 2
```
 
### 3. Sauvegarder et Rebooter
```plaintext
[HP] save
[HP] quit
<HP> reboot
```
 
### 4. Définir la Priorité de l'IRF
```plaintext
<HP> system-view
[HP] irf member 2 priority 31
```
 
 
### 5.Désactiver les Interfaces 10-Gigabit Ethernet
 
```plaintext
[HP] interface ten 2/1/1
[HP-Interface-Ten-GigabitEthernet2/1/1] shut
[HP-Interface-Ten-GigabitEthernet2/1/1] quit
[HP] interface ten 2/1/2
[HP-Interface-Ten-GigabitEthernet2/1/2] shut
[HP-Interface-Ten-GigabitEthernet2/1/2] quit
```
 
### 6. Configurer les Ports IRF
 
```plaintext
[HP] irf-port 2/1
[HP-irf-port2/1] port group interface Ten-GigabitEthernet 2/1/1
[HP-irf-port2/1] quit
[HP] irf-port 2/2
[HP-irf-port2/2] port group interface Ten-GigabitEthernet 2/1/2
[HP-irf-port2/2] quit
```
 
### 7. Réactiver les Interfaces
 
```plaintext
[HP] interface ten 2/1/1
[HP-Interface-Ten-GigabitEthernet2/1/1] undo shut
[HP-Interface-Ten-GigabitEthernet2/1/1] quit
[HP] interface ten 2/1/2
[HP-Interface-Ten-GigabitEthernet2/1/2] undo shut
[HP-Interface-Ten-GigabitEthernet2/1/2] quit
```
 
### 8. Activer la Configuration IRF
 
```plaintext
[HP] irf-port-configuration active
```
 
### 9. Sauvegarder et Rebooter
 
```plaintext
[HP] save
<HP> reboot
```
 
 
# Conclusion
 
Après avoir configuré les deux switches en suivant ces étapes, ils devraient former un stack. Une fois redémarrés, les switches devraient fonctionner ensemble en tant que seule entité logique. Utilise la commande display current-config pour vérifier que la configuration du stack est correcte et que les deux switches sont bien connectés et synchronisés.
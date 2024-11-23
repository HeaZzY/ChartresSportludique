# Configuration du Routeur de Management

Ce document décrit la configuration d'un routeur de management utilisant UFW (Uncomplicated Firewall). Le routeur dispose de deux interfaces réseau :  
- **Interface vers la DMZ privée** : `192.168.128.0/24`
- **Interface vers le réseau de management** : `10.10.240.0/24`

---

## Objectifs

1. Activer le NAT entre la DMZ et le réseau de management.
2. Bloquer l'ICMP vers le routeur pour masquer sa présence.
3. Désactiver SSH sur l'interface DMZ pour empêcher la découverte via Nmap.

---

## Configuration Réseau

### Interfaces
Configurez les interfaces réseau dans `/etc/network/interfaces` (ou avec Netplan si nécessaire).

```bash
# Interface DMZ
auto eth0
iface eth0 inet static
    address 192.168.128.1
    netmask 255.255.255.0

# Interface Management
auto eth1
iface eth1 inet static
    address 10.10.240.1
    netmask 255.255.255.0
```

Ces interfaces permettront au routeur de communiquer avec les deux réseaux.

---

## Configuration `iptables`

### Activer le NAT

Pour activer le NAT (Network Address Translation) et permettre aux hôtes dans la DMZ d'accéder au réseau de management, utilisez `iptables` pour configurer le masquerading.

1. Activez le forwarding IP :

```bash 
echo 1 > /proc/sys/net/ipv4/ip_forward

# ou 

sudo nano /etc/sysctl.conf
net.ipv4.ip_forward=1
sudo sysctl -p
```

2. Configurer les règles NAT avec `iptables` 

```bash
sudo iptables -t nat -A POSTROUTING -o ens4 -j MASQUERADE
```
Configurer le transfert des paquets entre ens3 (LAN) et ens4 (WAN) :
- Autorisez le trafic qui passe de ens3 (intérieur) à ens4 (extérieur) :
```bash
sudo iptables -A FORWARD -i ens3 -o ens4 -j ACCEPT
```
Autorisez les connexions retour pour maintenir les connexions établies :
```bash
sudo iptables -A FORWARD -i ens4 -o ens3 -m state --state RELATED,ESTABLISHED -j ACCEPT
```
3. Vérifier les règles : Pour voir les règles appliquées :
```bash
sudo iptables -L -v -n
sudo iptables -t nat -L -v -n
```

---

## Sauvegarder les règles iptables
Installez le paquet iptables-persistent pour rendre les règles permanentes :
```bash
sudo apt-get install iptables-persistent
```

Sauvegardez les règles actuelles :
```bash
sudo netfilter-persistent save
sudo netfilter-persistent reload
```

---
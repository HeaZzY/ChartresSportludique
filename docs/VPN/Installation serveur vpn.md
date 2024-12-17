# Installation d'un serveur vpn

### Installation d'OpenVPN et Easy-RSA :

```sudo apt install openvpn easy-rsa -y```

### Configuration du répertoire Easy-RSA : Créez un répertoire dédié pour les clés :


```bash
make-cadir ~/openvpn-ca
cd ~/openvpn-ca
```

### Initialisation de Easy-RSA :

```bash
./easyrsa init-pki
./easyrsa build-ca
```

### Création de la clé et du certificat du serveur :

```bash
./easyrsa gen-req server nopass
./easyrsa sign-req server server
```

### Génération des clés Diffie-Hellman :

```bash
./easyrsa gen-dh
```

#### Génération d'une ta key
```bash
openvpn --genkey --secret ta.key
```



### Voici la config openvpn pour un serveur

```conf
# Spécifie le port et le protocole utilisé

port 1194
proto udp
dev tun

# Emplacement des certificats et clés générés
ca /etc/openvpn/ca.crt
cert /etc/openvpn/server.crt
key /etc/openvpn/server.key
dh /etc/openvpn/dh.pem

# Activer l'authentification TLS
tls-auth /etc/openvpn/ta.key 0


# Réseau de clients OpenVPN
server 10.8.0.0 255.255.255.0
push "route 10.10.240.0 255.255.255.0"
# Permet aux clients d'accéder à internet via le VPN
# push "route 10.10.240.0 255.255.255.0"

# Configurations de sécurité
keepalive 10 120
comp-lzo
user nobody
group nogroup
persist-key
persist-tun

# Fichiers de log
status /var/log/openvpn-status.log
log /var/log/openvpn.log
verb 3

# Fichiers de persistances
ifconfig-pool-persist /etc/openvpn/ipp.txt

# Autoriser l'accès en tant qu'administrateur
client-to-client

topology subnet

#plugin /usr/lib/openvpn/openvpn-auth-ldap.so /etc/openvpn/auth-ldap.conf
#client-cert-not-required
#username-as-common-name
# Forcer l'authentification TLS
#auth-nocache

plugin /usr/lib/openvpn/openvpn-auth-ldap.so /etc/openvpn/auth-ldap.conf
#verify-client-cert none
```
# Ajout de serveur DMZ Publique

## Objectif
Configurer un serveur dans la DMZ publique avec deux passerelles : 
1. Une passerelle par défaut pointant vers `192.168.28.254`.
2. Une passerelle spécifique pour le réseau `172.28.96.0/19` pointant vers `192.168.28.10`.
3. Une autre passerelle pour la DMZ privée `192.168.128.0/24` pointant également vers `192.168.28.10`.

## Étapes de configuration

### 1. Configurer la passerelle par défaut
La passerelle par défaut est utilisée pour tout le trafic qui ne correspond pas aux routes spécifiques.

#### Commande Linux
```bash
sudo ip route add default via 192.168.28.254
```

2. Ajouter une route spécifique pour le réseau 172.28.96.0/19
Cette route est destinée au réseau interne.

## Commande Linux
```bash
sudo ip route add 172.28.96.0/19 via 192.168.28.10
```

3. Ajouter une route spécifique pour la DMZ privée 192.168.128.0/24
Cette route est utilisée pour la communication avec le réseau DMZ privée.

## Commande Linux
```bash
sudo ip route add 192.168.128.0/24 via 192.168.28.10
```

# Vérification des routes configurées

```bash
ip route
```

# Conclusion
Votre serveur est maintenant correctement configuré avec une passerelle par défaut pour le trafic externe, et deux routes spécifiques pour le réseau interne et la DMZ privée. Assurez-vous que les règles de pare-feu et de sécurité sont correctement appliquées pour protéger votre serveur dans la DMZ publique.
# 1. Configuration de TrueNAS  

## Informations Générales  
**Nom du Serveur** : `CHA-TRUENAS`  
**Version** : `TrueNAS-13.0-STABLE - Release Train for TrueNAS 13.0 [release]`  
**Adresse IP** : `192.168.128.115 - DMZ PRIVÉE`  
**Rôle** : Partage de fichiers, stockage en réseau (NAS)  
**Type** : Serveur Virtuel sur NUTANIX  

---

## Étapes de Configuration  

### 1. Installation  
1. Téléchargez l'image ISO de XigmaNAS depuis le site officiel.  
2. Importez l'ISO sur **Nutanix**.  
3. Créez la machine virtuelle avec les spécifications suivantes :  
   - **RAM** : 8 Go  
   - **Disque SYSTEM** : 50 Go  
   - **4 Disques DATA** : 500 Go chacun  

---

### 2. Configuration Réseau  

#### **Paramètres réseau**  
- **Adresse IP Statique** : `192.168.128.115`  
- **Masque de Sous-Réseau** : `255.255.255.0`  
- **Passerelle par défaut** : `192.168.128.254`  
- **DNS primaire** : `172.28.127.10`  
- **DNS secondaire** : `*`  
- **Enregistrement DNS** : `nas.chartres.sportludique.fr`  

#### **Accès au Tableau de Bord**  
- Ouvrez votre navigateur web et accédez à :  
  `http://192.168.128.115`  

---

### 3. Configuration des Services  

#### **SMB/CIFS** (Partage Windows)  
1. Accédez à `Services > SMB/CIFS`.  
2. Activez le service.  
3. Configurez les paramètres principaux :  
   - **Nom NetBIOS** : `LAN`  
   - **Nom de domaine** : `lan.chartres.sportludique.fr`  
4. Configurez les répertoires partagés pour chaque utilisateur du domaine (ex. `/home`).  

#### **SFTP** (Transfert de fichiers)  

1. **Activation du service**  
   - Activez le service dans `Services > FTP`.  

2. **Liaison des utilisateurs avec Kerberos**  
   - Configurez la liaison des utilisateurs avec Kerberos sur le serveur TrueNAS.  

3. **Authentification via Kerberos**  
   - Utilisez l'authentification Kerberos sur le port SSH du routeur, redirigé vers notre NAS.  

---

### 4. Gestion des Utilisateurs avec Active Directory  

L'intégration de TrueNAS avec un serveur **Active Directory (AD)** permet de centraliser la gestion des utilisateurs et des groupes, simplifiant l'administration des accès réseau.  

#### **Étapes pour configurer l'intégration avec Active Directory**  

1. **Pré-requis :**  
   - Un serveur Active Directory fonctionnel.  
   - Le domaine AD auquel TrueNAS doit se joindre (ex. `lan.chartres.sportludique.fr`).  
   - Un compte utilisateur AD avec des droits d'administration pour joindre des machines au domaine.  
   - TrueNAS configuré avec une adresse IP statique (ex. `192.168.128.115`).  

2. **Configurer les paramètres réseau :**  
   - Assurez-vous que TrueNAS peut résoudre les noms DNS du domaine AD.  
   - Configurez le DNS primaire de TrueNAS pour pointer vers le contrôleur de domaine AD (ex. `172.28.127.10`).  

   **Exemple :**  
   - Allez dans `Réseau > Configuration Globale`.  
   - Définissez le **DNS principal** comme l'IP de votre contrôleur de domaine.  
   - Redémarrez les services réseau pour appliquer les modifications.  

3. **Rejoindre le domaine Active Directory**  
   - Accédez à `Services > Active Directory`.  
   - Activez le service et remplissez les champs suivants :  
     - **Nom du Domaine** : `lan.chartres.sportludique.fr`  
     - **Nom NetBIOS** : `LAN`  
     - **Nom d'Utilisateur** : Un compte administrateur AD (ex. `admin`)  
     - **Mot de Passe** : Le mot de passe de l’utilisateur  
   - Cliquez sur **Enregistrer et Appliquer**.  

---

#### **Bonnes pratiques :**  
- Utilisez un compte de service dédié pour joindre TrueNAS au domaine, avec des droits limités.  
- Vérifiez que l'heure système de TrueNAS est synchronisée avec celle de votre contrôleur de domaine (utilisez **NTP** si nécessaire).  
- Surveillez régulièrement l'intégration pour détecter tout problème de connectivité ou de synchronisation.  

*Cette section assure une gestion optimisée et centralisée des utilisateurs à travers Active Directory dans TrueNAS.*  

---

## Informations Complémentaires  
- **Documentation officielle** : [https://www.truenas.com/](https://www.truenas.com/)  
- **Communauté et Support** : Forums sur le site officiel.  
# 1. Configuration de XigmaNAS

## Informations Générales
**Nom du Serveur**: `CHA-NAS`  
**Version**: `Dernière version stable - 13.3.0.5 - Hesterion (revision 10153)`  
**Adresse IP**: `192.168.128.112 - DMZ PRIVÉE`  
**Rôle**: Partage de fichiers, stockage en réseau (NAS)
**Type**: Serveur Virtuel sur NUTANIX
---

## Étapes de Configuration

### 1. Installation
1. Téléchargez l'image ISO de XigmaNAS depuis le site officiel.
2. Mettre l'iso sur **Nutanix**.
3. Création de la machine virtuelle.
- 8Go RAM 
- 1 Disque SYSTEM de 50Go
- 3 Disques DATA de 1To

---

### 3. Configuration Réseau

#### **Paramètres réseau**
- **Adresse IP Statique** : `192.168.128.112`
- **Masque de Sous-Réseau** : `255.255.255.0`
- **Passerelle par défaut** : `192.168.128.254`
- **DNS primaire** : `172.28.127.120`
- **DNS secondaire** : `*`

- **Enregistrement DNS**: nas.chartres.sportludique.fr

#### **Accès au Tableau de Bord**
- Ouvrez votre navigateur web et accédez à :  
  `http://192.168.128.112`

---

### 4. Configuration des Services - **PAS ENCORE ACTIF**
Activez et configurez les services nécessaires dans le tableau de bord :

#### **SMB/CIFS** (Partage Windows)
- Allez dans `Services > SMB/CIFS`.
- Activez le service.
- Configurez les paramètres principaux :
  - **Nom NetBIOS** : `XigmaNAS`
  - **Groupe de Travail** : `WORKGROUP`
- Ajoutez les répertoires partagés.

#### **NFS** (Partage UNIX/Linux)
- Activez le service dans `Services > NFS`.
- Configurez les chemins d'accès et permissions.

#### **FTP** (Transfert de fichiers)
- Activez le service dans `Services > FTP`.
- Créez des utilisateurs et attribuez des permissions.

---

### 5. Gestion des Utilisateurs avec Active Directory **PAS ENCORE ACTIF**

L'intégration de XigmaNAS avec un serveur **Active Directory (AD)** permet de centraliser la gestion des utilisateurs et des groupes, en simplifiant l'administration des accès réseau.

#### **Étapes pour configurer l'intégration avec Active Directory :**

1. **Pré-requis :**
   - Un serveur Active Directory fonctionnel.
   - Le domaine AD auquel XigmaNAS doit se joindre (ex. `chartres.sportludique.fr`). 
   - Un compte utilisateur dans l'AD avec des droits d'administration pour joindre des machines au domaine.
   - XigmaNAS configuré avec une adresse IP statique (ex. `192.168.128.112`).

2. **Configurer les paramètres réseau :**
   - Assurez-vous que XigmaNAS peut résoudre les noms DNS du domaine AD.
   - Configurez le DNS primaire de XigmaNAS pour pointer vers le contrôleur de domaine AD (ex. `172.28.127.120`).

   **Exemple :**
   - Allez dans `Réseau > Configuration Globale`.
   - Définissez le **DNS principal** comme l'IP de votre contrôleur de domaine.
   - Redémarrez les services réseau pour appliquer les modifications.

3. **Rejoindre le domaine Active Directory :**
   - Accédez à `Services > Active Directory`.
   - Activez le service et remplissez les champs :
     - **Nom du Domaine** : `lan.chartres.sportludique.fr`.
     - **Nom NetBIOS** : `xigmanas`.
     - **Nom d'Utilisateur** : Un compte administrateur AD (ex. `admin`).
     - **Mot de Passe** : Le mot de passe de l’utilisateur.
     - **Serveur LDAP** (optionnel) : Laissez vide pour une détection automatique.
     - **Serveur Kerberos** : Laissez vide pour une détection automatique.
   - Cliquez sur **Enregistrer et Appliquer**.

4. **Vérifier l'intégration :**
   - Une fois connecté au domaine, les utilisateurs et groupes AD seront visibles dans `Accès > Utilisateurs et Groupes`.
   - Testez la connexion en vous connectant avec un utilisateur AD à un partage réseau configuré.

5. **Configurer les permissions avec les groupes AD :**
   - Allez dans `Disques > Partages`.
   - Ajoutez un partage ou modifiez un existant.
   - Dans la section **Permissions**, sélectionnez un utilisateur ou un groupe provenant d’AD (ex. `DOMAIN\Utilisateurs`).
   - Définissez les permissions :
     - **Lecture seule** : Les membres peuvent uniquement voir les fichiers.
     - **Lecture/Écriture** : Les membres peuvent modifier ou supprimer des fichiers.
   - Enregistrez les modifications.

6. **Surveiller l’état de l’intégration :**
   - Consultez les journaux de XigmaNAS via `Système > Journaux` pour vérifier l'état des connexions AD.
   - En cas de problème, vérifiez la configuration DNS et assurez-vous que l'heure système est synchronisée avec le contrôleur AD.

---

#### **Bonnes pratiques :**
- Utilisez un compte de service dédié pour rejoindre XigmaNAS au domaine, avec des droits limités.
- Vérifiez que l'heure système de XigmaNAS est synchronisée avec celle de votre contrôleur de domaine (utilisez **NTP** si nécessaire).
- Limitez les permissions des partages réseau en utilisant des groupes AD pour une gestion plus simple.
- Surveillez régulièrement l'intégration pour détecter tout problème de connectivité ou de synchronisation.

*Cette section assure une gestion optimisée et centralisée des utilisateurs à travers Active Directory dans XigmaNAS.*


---

### 6. Surveillance et Maintenance
- Configurez les notifications par email pour les alertes système.
- Vérifiez régulièrement les journaux système dans `Système > Journaux`.

---

## Informations Complémentaires
- **Documentation officielle** : [https://www.xigmanas.com/wiki/](https://www.xigmanas.com/wiki/)  
- **Communauté et Support** : Forums sur le site officiel.

---

*Dernière mise à jour de la configuration : [20/11/2024]*

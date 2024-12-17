# Mise en Place du Partage SMB/CIFS sur TrueNAS  

## 1. Introduction  

Le protocole **SMB/CIFS** (Server Message Block) permet de partager des fichiers entre le serveur **TrueNAS** et des machines sous Windows. Cette documentation explique étape par étape la configuration du partage SMB sur un serveur TrueNAS.  

---

## 2. Prérequis  

Avant de commencer, assurez-vous d'avoir les éléments suivants :  

- **TrueNAS installé et accessible** via son interface web.  
- **Adresse IP statique configurée** sur le serveur.  
- **Un domaine Active Directory (AD)** si vous souhaitez gérer les utilisateurs via un domaine.  
- **Des dossiers à partager** créés sur le système de fichiers de TrueNAS.  

---

## 3. Étapes de Configuration  

### **3.1. Configuration du Service SMB**  

1. Connectez-vous à l'interface web de **TrueNAS**.  
2. Allez dans le menu **Services**.  
3. Trouvez le service **SMB** (ou SMB/CIFS) dans la liste.  
4. Activez le service en basculant l'interrupteur sur **ON**.  

---

### **3.2. Configuration des Paramètres Principaux**  

1. Accédez à **Services > SMB > Configuration**.  
2. Remplissez les champs suivants :  

   - **Nom NetBIOS** : `truenas` (ou un nom de serveur lisible)  
   - **Nom de Domaine** : `lan.chartres.sportludique.fr`  
   - **Description** : `Partage de fichiers pour les utilisateurs`  
   - **Serveur de Travail** : `LAN` (ou votre groupe de travail)  
   - **Serveur de Temps (Time Server)** : Synchronisé avec un serveur NTP  

3. Cliquez sur **Enregistrer** pour appliquer les paramètres.  

---

### **3.3. Création des Répertoires Partagés (Windows Share SMB)**  

1. Accédez au menu **Sharing > Windows Shares (SMB)**.  
2. Cliquez sur **Add** pour ajouter un nouveau partage.  

3. **Configuration de base** :  
   - **Path** : Sélectionnez le chemin vers le répertoire à partager (ex. `/mnt/storage-1`).  
   - **Name** : Donnez un nom au partage (ex. `home`).  

4. **Paramètres spécifiques** :  
   - **Purpose** : Sélectionnez **Multi-User Time Machine**.  
   - **Description** : Ajoutez une description pour identifier le partage (ex. *Création des homes de utilisateurs*).  

5. Cliquez sur **Save** pour enregistrer et appliquer la configuration.  

---

## 4. Bonnes Pratiques  

- **Sécurité** :  
  - N'autorisez pas l'accès anonyme (invité).  
  - Utilisez **Active Directory** pour centraliser la gestion des utilisateurs.  
- **Permissions** :  
  - Configurez des permissions précises pour chaque dossier partagé.  
  - Limitez les accès en lecture/écriture aux utilisateurs nécessaires.  
- **Sauvegarde** :  
  - Assurez-vous que vos partages SMB sont sauvegardés régulièrement.  

---

## 5. Conclusion  

La configuration du service SMB sur TrueNAS permet de centraliser et de sécuriser le partage de fichiers au sein de votre réseau. Grâce aux permissions adaptées et à l'intégration avec Active Directory, vous pouvez facilement gérer les accès utilisateurs tout en assurant une bonne sécurité des données.  

---

## 6. Références  

- [Documentation officielle TrueNAS](https://www.truenas.com/docs/)  
- [SMB Protocol - Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/storage/file-server/smb-overview)  

# Configuration de Microsoft NPS comme Serveur RADIUS sur Active Directory

## Prérequis
- Un serveur **Windows Server** avec **Active Directory (AD)** installé et configuré.
- Un serveur **NPS (Network Policy Server)** installé.
- Un serveur **AD CS (Active Directory Certificate Services)** pour la gestion des certificats.
- Une **borne WiFi** ou tout autre périphérique réseau compatible RADIUS.
- Un **certificat** pour le chiffrement de l’authentification RADIUS.

---

## 1. Installation de NPS sur Windows Server

### 1.1 Ajouter le rôle NPS
1. **Ouvrir le Gestionnaire de serveur** (`Server Manager`).
2. Aller dans **Gérer > Ajouter des rôles et fonctionnalités**.
3. Sélectionner **Installation basée sur un rôle ou une fonctionnalité**.
4. Choisir le serveur local.
5. Dans **Rôles de serveurs**, cocher **Network Policy and Access Services** et cliquer sur **Suivant**.
6. Dans **Services de rôle**, cocher **Network Policy Server (NPS)**.
7. Terminer l’installation et redémarrer le serveur si nécessaire.

![Description de l'image](images/1.png)

---

## 2. Installation et Configuration de AD CS (Active Directory Certificate Services)

### 2.1 Ajouter le rôle AD CS
1. **Ouvrir le Gestionnaire de serveur** (`Server Manager`).
2. Aller dans **Gérer > Ajouter des rôles et fonctionnalités**.
3. Sélectionner **Active Directory Certificate Services (AD CS)**.
4. Installer les services suivants :
   - **Autorité de certification (CA)**
   - **Service d'inscription Web pour les certificats** (si nécessaire)
5. Finaliser l'installation et configurer une **autorité de certification d'entreprise**.

---

## 3. Enregistrement de NPS dans Active Directory
1. Ouvrir la console **NPS** (`nps.msc`).
2. Dans le panneau de gauche, **clic droit sur NPS (Local) > Enregistrer le serveur dans Active Directory**.
3. Confirmer l’action.

---

## 4. Ajouter un Client RADIUS (ex: Borne WiFi)
1. Ouvrir la console **NPS** (`nps.msc`).
2. Dans le panneau de gauche, **développer RADIUS Clients et Serveurs > Clic droit sur Clients RADIUS > Nouveau**.
3. **Remplir les informations** :
   - Nom convivial : `Borne_WiFi`
   - Adresse IP ou Nom DNS : **Adresse IP de la borne WiFi**
   - Clé secrète partagée : **Définir une clé et l’enregistrer pour la borne**
4. Valider avec **OK**.

---

## 5. Création d'une Stratégie de Connexion RADIUS

### 5.1 Ajouter une nouvelle stratégie de connexion
1. Dans la console **NPS**, aller dans **Politiques > Stratégies de connexion réseau**.
2. **Clic droit > Nouvelle stratégie**.
3. **Nommer la stratégie** (`WiFi-RADIUS`).
4. **Définir les conditions** :
   - **Client RADIUS** : sélectionner la borne WiFi précédemment ajoutée.
   - **Groupes Windows** : ajouter un groupe d’utilisateurs AD autorisés (`WiFi Users` par exemple).
5. Terminer la configuration et appliquer les changements.


---

## 8. Vérification et Dépannage

### 8.1 Vérifier les logs NPS
- Ouvrir l’Observateur d’événements (`eventvwr.msc`).
- Aller dans **Journaux Windows > Sécurité** et chercher les événements NPS.

### 8.2 Tester avec différents utilisateurs
- Vérifier que seuls les utilisateurs AD autorisés peuvent se connecter.

### 8.3 Vérifier la connectivité entre la borne WiFi et le serveur RADIUS
- Tester la communication avec la commande :  
  ```powershell
  ping <IP_du_serveur_RADIUS>


---




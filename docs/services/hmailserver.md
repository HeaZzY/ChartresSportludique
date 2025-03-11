# Configuration de hMailServer sur Windows

## 1. Installation de hMailServer

1. **Téléchargez hMailServer** : Rendez-vous sur [hMailServer](https://www.hmailserver.com/) et téléchargez l'installateur.
2. **Installez hMailServer** :
   - Lancez l'installateur et suivez les instructions.
   - Choisissez la base de données interne ou une base de données externe.
3. **Configuration initiale** :
   - Ouvrez **hMailServer Administrator**.
   - Connexion avec le mot de passe administrateur défini.


### 2 Création des Comptes Utilisateurs

- Création d'un fichier en format CSV avec les différents utilisateur de l'entreprise et avec un mots de passe par défault pour tous le monde.
- Création d'un script PowerShell pour implenter les utilisateurs à hmailserver.

```powershell
#**************************************************
#
#    Création de comptes de messagerie dans hMailServer
#
#**************************************************

# Définir le chemin du fichier CSV
$csvFilePath = "C:\Users\Admin\Downloads\chartres2.csv"  # Remplacez par le chemin de votre fichier CSV

# Charger les utilisateurs depuis le fichier CSV
$users = Import-Csv -Path $csvFilePath -Delimiter ";"

# Configuration du serveur de messagerie
$dnsroot = "chartres.sportludique.fr"  # Domaine utilisé pour les adresses mail

# Initialisation de la connexion hMailServer
$hm = New-Object -ComObject hMailServer.Application

# Authentification en tant qu'administrateur
$hm.Authenticate("Administrator", "Hmail28@") | Out-Null

# Sélectionner le domaine dans hMailServer
$hmdom = $hm.Domains.ItemByName($dnsroot)

# Boucle pour créer un compte pour chaque utilisateur dans le fichier CSV
foreach ($user in $users) {
    # Générer l’adresse e-mail à partir des informations de l'utilisateur
    $emailAddress = "$($user.NOM).$($user.PRENOM)@$dnsroot"
    $username = "$($user.NOM).$($user.PRENOM)"

    # Création du compte
    $hmact = $hmdom.Accounts.Add()

    # Attributs du compte
    $hmact.Address = $emailAddress
    $hmact.Active = $true
    $hmact.IsAD = $false
    $hmact.MaxSize = 100
    $hmact.ADUsername = $username
    $hmact.PersonFirstName = $user.PRENOM
    $hmact.PersonLastName = $user.NOM

    # Définir le mot de passe par défaut
    $hmact.Password = "azerty123@"

    # Enregistrer le compte
    $hmact.Save()

    Write-Host "Compte créé pour $($user.PRENOM) $($user.NOM) avec l'adresse $emailAddress"
}

Write-Host "Tous les comptes ont été créés avec succès."
```

## 3. Configuration SMTP sur le Port 587

### 3.1 Configuration du Port SMTP

- Allez dans **Settings** > **Protocols** > **SMTP** > **Advanced**.
- Dans **TCP/IP ports**, ajoutez le port 587 :
  - Cliquez sur **Add**.
  - **Port** : 587
  - **Type** : SMTP
  - **Connection Security** : STARTTLS (pour la sécurisation).
- Cliquez sur **Save**.

### 3.2 Configuration des Règles de Pare-feu

- Ouvrez **Windows Defender Firewall**.
- Allez dans **Advanced settings** > **Inbound Rules** > **New Rule**.
- Sélectionnez **Port**, entrez **587,25,143**, et autorisez la connexion.
- Appliquez à tous les profils (Domaine, Privé, Public).
- Nommez la règle et cliquez sur **Finish**.

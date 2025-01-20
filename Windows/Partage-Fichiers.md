# Quête-Partage-de-fichiers-Windows

## :one: Installation du rôle Partage de fichiers  
  
Dans le cas où le Partage de fichiers n'est pas déjà installé sur le serveur, suivre les étapes suivantes :  
  
➡️ Sur le Server Manager, cliquer sur `Add roles and features`  
  
➡️ Cliquer sur `Next` 3 fois jusqu'à arriver à la fenêtre de séléction de rôles  
  
➡️ Séléctionner `File and Storage Services` et cliquer sur `Next`  
  
➡️ Suivre les étapes jusqu'à la fin de l'installation  

## :two: Création d'un partage  
  
Maintenant que nous avons installé le rôle, nous allons créer un nouveau partage.  
  
Avant toute chose, rendez-vous sur C:\ et créez un dossier `Documents_Entreprise`  
  
➡️ Se rendre sur Server Manager  
  
➡️ Dans le menu de gauche, cliquer sur `File and Storage Services` puis sur `Shares`  
  
➡️ Faire un clic droit sur la fenêtre et cliquer sur `New Share...`  
  
➡️ Choisir `SMB Share - Quick` et `Next`  
  
➡️ Pour la localisation du partage, nous allons cocher `Type a custom path` et entrer le chemin suivant : `C:\Documents_Entreprise`  
  
➡️ En face de `Share name:`, entrer le nom `Docs`  
  
➡️ Cliquer sur `Next` jusqu'à terminer l'installation  

## :three: Création des utilisateurs dans l'Active Directory  
  
Nous avons besoin de créer des utilisateurs dans différents groupes pour tester si les permissions configurées fonctionnenent bien.  
  
Suivez ces étapes :  
  
➡️ Server Manager -> **Tools** -> **Active Directory Users and Computers**  
  
➡️ Clic droit sur le nom de domaine puis cliquer sur `New` -> `Organizational Unit`  
  
➡️ Répéter l'opération 3 fois avec comme noms `RH`, `Comptabilité` et `Direction`  
  
➡️ Dans chaque OU, créer un nouveau groupe que l'on appellera `Users RH` `Users Compta` `Users Dir`
  
## 4️⃣ Configuration des permissions pour les sous dossiers en fonction des groupes  
  
Créez trois sous dossiers sous `Documents_Entreprise` : `RH` `Comptabilité` et `Direction`  
  
Se rendre dans l'explorateur de fichiers, `This PC` -> `Local Disk (C:)` -> `Documents_Entreprise`  
  
💸 **Comptabilité**  
Faire un clic droit sur le nom du dossier **Comptabilité** puis `Properties`. Dans l'onglet `Security`, cliquer sur le bouton 'Edit' puis 'Add'  
Rechercher le nom du groupe (ici, c'est `Users Compta`)    
Une fois le groupe ajouté, modifier ses droits et cocher **Allow** pour **Read** et **Write**  

👨‍👨‍👦‍👦 **RH**  
Faire un clic droit sur le nom du dossier **RH** puis `Properties`. Dans l'onglet `Security`, cliquer sur le bouton 'Edit' puis 'Add'  
Rechercher le nom du groupe (ici, c'est `Users RH`)  
Une fois le groupe ajouté, modifier ses droits et cocher **Allow** pour **Read** et **Write**  

👔 **Direction**  
Pour ce groupe, nous allons attribuer les droits d'écriture et de lecture sur les dossiers **Comptabilité**, **RH** et **Direction**.  
La procédure est la même que pour les groupes précédents.  
Le nom du groupe à rechercher sur les trois dossiers est `Users Dir`  

## :five: Configuration des permissions pour le dossier racine Documents_Entreprise  
  
Lorsqu'on essaye de changer les permissions de la même manière que nous l'avons fait pour les sous-dossiers, on voit que les cases Allow sont grisées et ne peuvent être décochées. Cela s'explique par le fait que les permissions actuelles ont été héritées d'un parent. Pour résoudre ce problème :  
  
➡️ Clic droit sur le dossier `Documents_Entreprise` -> `Properties` -> `Security` -> `Advanced`  
  
➡️ Cliquer sur le bouton `Disable inheritance` -> **Convert inherited permissions into explicit permissions**  
  
Les permissions peuvent maintenant être modifiées, les cases n'étant plus grisées.  
  
Sur le dossier `Documents_Entreprise`, mettre les droits de lecture à tous les utilisateurs du domaine : **Users (WILDERS\Users)**  
  
## :six: Lister les partages sur le serveur  
  
Ouvrir PowerShell. Taper la commande suivante : `Get-SmbShare`  
On a bien une ligne qui apparaît avec C:\Documents_Entreprise  
  
## :seven: Configuration d'un lecteur réseau sur client  
  
Se connecter avec un utilisateur faisant partie d'un des 3 groupes crées précédemment.  
Ici, c'est un utilisateur du groupe **Direction** qui a été utilisé.  
Ouvrir un PowerShell en administrateur et taper la commande suivante : `New-PSDrive -Name "Z" -PSProvider FileSystem -Root "\\SRVWIN01\Docs" -Persist`  
Ensuite, ouvrir l'explorateur de fichiers, clic droit sur **This PC** puis cliquer sur `Map network drive...`  
Une fenêtre s'ouvre. Entrer le chemin du dossier, soit `\\SRVWIN01\Docs`  
Désormais, le fichier partagé s'affiche dans l'explorateur de fichiers.  
  
## :eight: Tests des configurations  
  
Dans l'AD, créer un compte pour chaque groupe et tester les permissions.

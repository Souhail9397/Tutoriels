# :one: Installation du service SSH  
  
Nous allons avoir besoin d'installer SSH sur deux machines : l'une qui va fournir le service SSH (serveur), et l'autre qui va en prendre le contrôle (client)  
  
💻 **Sur machine client**  
  
➡️ Taper `Apps and features` dans la barre de recherche  
  
➡️ Cliquer sur `Optional features` puis sur `Add a feature`  
  
➡️ Rechercher et installer `OpenSSH Client`  
  
🖥️ **Sur machine serveur**  
  
➡️ Taper `Apps and features` dans la barre de recherche  
  
➡️ Cliquer sur `Optional features` puis sur `Add a feature`  
  
➡️ Rechercher et installer `OpenSSH Server`  
  
➡️ Ouvrir PowerShell en administrateur et taper la commande `get-service sshd`  
  
➡️ Cette commande nous retourne que le service SSH est "Stopped". Pour l'activer automatiquement au démarrage, taper la commande `get-service sshd | set-service -startuptype automatic`  
  
➡️ Redémarrer ensuite le service avec la commande `Restart-Service sshd`  
  
➡️ En revérifiant le statut du service sshd avec la commande `get-service sshd`, on remarque que le statut est passé à "Running"  
  
# 2️⃣ Génération de clé SSH    
  
💻 **Sur machine client**  
  
➡️ Ouvrir PowerShell et taper la commande `ssh-keygen -t ecdsa`   
  
➡️ **Enter file in which to save the key** : appuyer sur Entrée  
  
➡️ **Enter passphrase** : appuyer sur Entrée  
  
![image](https://github.com/user-attachments/assets/a5503d0a-b7fd-4d7c-a81e-50d4b788a263)  
  
Nous allons maintenant vérifier si la clé a bien été créée et est au bon endroit : 
  
➡️ Taper `Set-Location` avec le chemin indiqué lors de la création de la clé pour vérifier la création de la clé ssh   
  
➡️ Taper `Get-ChildItem`  
  
![image](https://github.com/user-attachments/assets/42d24d7c-7cb6-4356-9284-05d296c3135c)  
  
# :three: Test de la connexion SSH sur le serveur  
  
💻 **Sur machine client**  
  
➡️ Taper la commande `ssh utilisateur@adresseip`  
  
➡️  **Are you sure you want to continue connecting?** : yes  
  
➡️ **utilisateur@adresseip's password** : entrer le mot de passe de l'utlisateur sur lequel on essaye de se connecter  

![image](https://github.com/user-attachments/assets/c7dea3f8-ffcf-4b93-8b42-9a7f57e25390)

  
# :four: Déploiement de la clé SSH public sur le serveur  
  
🖥️ **Sur machine serveur**  
  
Créer, dans l'espace personnel de l'utilisateur, un dossier **.ssh** et dans ce dossier, créer un fichier **authorized_keys**  
  
💻 **Sur machine client**  
  
Avant de commencer, quitter la connexion ssh en tapant `exit` : nous voilà de retour sur notre machine cliente  
  
➡️ S'assurer qu'on se trouve bien sur notre espace personnel : `Set-Location ~`  
  
➡️ Afficher la clé publique avec la commande `Get-Content -path .\.ssh\id_ecdsa.pub` afin de pouvoir la copier   
  
![image](https://github.com/user-attachments/assets/65315160-aac7-4f63-bb3c-3b65f3d192f1)  
  
➡️ Se reconnecter sur le serveur (**et sur PowerShell**) avec la commande `ssh utilisateur@adresseip powershell`  
  
![image](https://github.com/user-attachments/assets/ff8cd2d5-ef44-433c-9e64-16d6f8a8483b)  
  
➡️ Taper la commande `add-content -path .ssh\authorized_keys -value "clé ssh"`

![image](https://github.com/user-attachments/assets/0584d525-ae9f-420c-8bfe-b40e869123ff)  
  
➡️ En vérifiant le fichier texte qu'on a créé au préalable, on y retrouve notre clé ssh  
  
![image](https://github.com/user-attachments/assets/a9683060-8437-42ed-b4a6-b5bb94d29f08)  
  
# :five: Modification du fichier de configuration  
  
Malgré les manipulations effectuées précédemment, la connexion sans mot de passe n'est toujours pas possible. Cela s'explique par l'existence d'un fichier de configuration que nous n'avons pas encore modifié.  
  
🖥️ **Sur machine serveur**  
  
➡️ Ouvrir PowerShell en Admin et taper `C:\ProgramData\ssh\sshd_config`  
  
➡️ Ouvrir le fichier avec le bloc notes via la fenêtre qui s'affiche afin de pouvoir le modifier  
  
➡️ Commenter la denrière ligne :  
  
![image](https://github.com/user-attachments/assets/33af77c9-2a95-480f-8469-e167ba9a530c)  
  
➡️ Sauvegarder et quitter, puis redémarrer le service ssh avec la commande `Restart-service sshd`  
  
➡️ Renteter la connexion ssh, et le mot de passe devrait maintenant ne plus être demandé :  
  
![image](https://github.com/user-attachments/assets/232c44e5-d6fb-4ccb-9f20-7ad12f27ac68)   
  

  


  


  


  






  



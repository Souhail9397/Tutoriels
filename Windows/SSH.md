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
  
![image](https://github.com/user-attachments/assets/cb65ef4c-0170-429b-992b-05d49c9460a1)  
  
Nous allons maintenant vérifier si la clé a bien été créée et est au bon endroit :  
  
![image](https://github.com/user-attachments/assets/fbc7bf6f-90fa-4531-9a56-2eb57c524052)  
  
➡️ Taper `Set-Location C:\Users\Administrator\.ssh`
  
# :three: Test de la connexion SSH sur le serveur  
  
💻 **Sur machine client**  
  
➡️ Taper la commande `ssh utilisateur@adresseip`  
  
➡️  **Are you sure you want to continue connecting?** : yes  
  
➡️ **utilisateur@adresseip's password** : entrer le mot de passe de l'utlisateur sur lequel on essaye de se connecter  

![image](https://github.com/user-attachments/assets/c7dea3f8-ffcf-4b93-8b42-9a7f57e25390)

  
# :four: Déploiement de la clé SSH public sur le serveur  
  
Avant de commencer, quitter la connexion ssh en tapant `exit` : nous voilà de retour sur notre machine cliente  
  
➡️ S'assurer qu'on se trouve bien sur notre espace personnel : `Set-Location ~`  
  
➡️ Afficher la clé publique avec la commande `Get-Content -path .\.ssh\id_ecdsa.pub` afin de pouvoir la copier   
  
![image](https://github.com/user-attachments/assets/65315160-aac7-4f63-bb3c-3b65f3d192f1)  
  
➡️ Se reconnecter sur le serveur (**et sur PowerShell**) avec la commande `ssh utilisateur@adresseip powershell`  
  
![image](https://github.com/user-attachments/assets/ff8cd2d5-ef44-433c-9e64-16d6f8a8483b)  
  
➡️ Taper la commande `add-content -path .ssh\authorized_keys -value "clé ssh"`
  ......





  



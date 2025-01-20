# Installer PfSense  
  
## :one: Installation de PfSense  
  
### Étape 1 : Télécharger l’image ISO  

Téléchargez l’image ISO de PfSense depuis l’un des sites suivants :  
  
https://pfsense.org/download/  
https://repo.ialab.dsu.edu/pfsense/  
  
### Étape 2 : Configuration de la machine virtuelle  
  
Créez une machine virtuelle (VM) avec les paramètres suivants :  
  
➡️ **Nom** : Firewall PfSense  
  
➡️ **Type** : BSD  
  
➡️ **Version** : FreeBSD (64-bit)  
  
➡️ **Mémoire** : 1024 Mo  
  
➡️ **Disque dur** : Minimum 16 Go (format VDI)  
  
➡️ **Réseau** :  
- Une interface en Accès par pont  
- Une interface en Réseau interne (nom : LAN_VM)  
  
➡️ Insérez ensuite l’image ISO téléchargée, puis lancez la VM pour démarrer l’installation.  

### Étape 3 : Installation de PfSense  
  
➡Lors de l’installation, suivez les étapes suivantes :  
  
➡️ **Copyright and distribution notice** : Acceptez les termes  

➡️ **Welcome to PfSense** : Sélectionnez `Install`  
  
➡️ **Keymap Selection** : Choisissez `French (accent keys)`   
  
➡️ **Partitioning** : Sélectionnez `Auto (ZFS)`    
  
➡️ **ZFS Configuration** : Acceptez les options par défaut et choisissez `stripe`  
  
➡️ **Sélectionnez le disque à utiliser** : appuyez sur la barre d’espace pour cocher `ada0`, puis validez  
  
➡️ Confirmez la configuration avec `Yes`  
  
Patientez pendant l’installation. Une fois terminée :  
  
➡️ **Manual Configuration** : Choisissez `No`  
  
➡️ **Complete** : Redémarrez la machine  
  
⚠️ Avant le redémarrage, retirez l’image ISO du lecteur de disque virtuel  
  
### Étape 4 : Configuration des interfaces  
  
Lorsque la machine redémarre, les adresses des interfaces s’affichent  
  
Si les deux interfaces se trouvent sur le même réseau, vous devez attribuer une adresse différente à l’interface LAN en utilisant le menu de PfSense
 
### Étape 5 : Configuration de la machine cliente  
  
Ajoutez une machine cliente (par exemple, Windows 10) et configurez une carte réseau en Réseau interne avec le même nom : LAN_VM  
  
Configurez une adresse IP statique pour cette machine, dans le même réseau que l’interface LAN de PfSense  
  
🧰 Exemple de configuration :  
  
➡️ **Adresse IP** : 192.168.1.2/24  
  
➡️ **Passerelle** : 192.168.1.1  
  
➡️ **Serveur DNS préféré** : 1.1.1.1  
  
# 2️⃣ Configuration de PfSense  
  
### Étape 1 : Accéder à l’interface graphique  
  
Sur la machine cliente, ouvrez un navigateur et rendez-vous sur `https://192.168.1.1`  
  
Un message peut apparaître, indiquant que la connexion n’est pas privée. Cliquez sur `Avancé`, puis sur `Continuer vers le site`  
  
### Étape 2 : Connexion et configuration initiale  
  
Connectez-vous à PfSense avec les identifiants par défaut :  
  
- **Utilisateur** : admin  
  
- **Mot de passe** : pfsense  
  
Ensuite, procédez à la configuration initiale :  
  
➡️ Cliquez deux fois sur `Next`  
   
➡️ Configurez les paramètres généraux :  
  
- **Nom du serveur** : PfSense (ou autre, selon votre choix)  
  
- **Nom de domaine** : Optionnel  
  
- **Serveur DNS principal** : 1.1.1.1  
  
Cliquez sur `Next`  
  
➡️ Configurez l’heure :  
  
- **Serveur de temps** : fr.pool.ntp.org  
  
- **Fuseau horaire** : Europe/Paris  
  
➡️ Cliquez sur `Next`  
  
➡️ Configurez l’interface WAN :  
  
- **Type sélectionné** : DHCP  
  
- Décochez : Block private networks from entering via WAN  
  
- Décochez : Block non-Internet routed networks from entering via WAN  
  
➡️ Cliquez sur `Next`   
  
➡️ Configurez l’interface LAN (déjà configurée automatiquement)  
  
➡️ Cliquez sur `Next`    
  
➡️ Définissez un mot de passe pour l’administrateur, puis cliquez sur `Next`    
  
➡️ Cliquez sur `Reload`, puis sur `Finish`  
  
PfSense est maintenant opérationnel  
  
### Étape 3 : Tester la connectivité  
  
Sur la machine cliente, ouvrez une invite de commande et tentez de pinguer une adresse externe : `ping 8.8.8.8`  
  
✔️ Si le test est concluant, essayez d’accéder à un site web (par exemple, Google). Cela devrait fonctionner.  
  
# 3️⃣ Ajouter une règle de filtrage réseau  
  
### Étape 1 : Bloquer l’accès à l’extérieur  
  
Pour interdire à la machine cliente de sortir du réseau interne :  
  
Rendez-vous dans l’interface web de PfSense :  
  
➡️ Firewall > Rules > LAN  
  
➡️ Cliquez sur Add et configurez la règle comme suit :  
  
- **Action** : Block  
  
- **Protocole** : Any  
  
- **Source** : Adresse (ou alias) - 192.168.1.2  
  
- **Destination** : Any  
  
- **Description** : Bloquer l’accès de la machine à l’extérieur  
  
Enregistrez la règle `Save` et appliquez les changements `Apply changes`  
  
### Étape 2 : Tester la règle  
  
🖥️ Sur la machine cliente  
  
Essayez de pinguer une adresse externe (par exemple, `8.8.8.8`) : cela ne fonctionnera pas.  
  
Essayez d’accéder à un site web : cela ne fonctionnera pas non plus.  
  
🖥️ Sur le poste d’administration  
  
Dans le shell de PfSense (option 8 - Shell), pinguez une adresse externe (par exemple, `8.8.8.8`)  
  
✔️ Le test fonctionnera, confirmant que le filtrage s’applique uniquement à la machine cliente.

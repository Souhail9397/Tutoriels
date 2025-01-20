# :one: Création du serveur DHCP  
  
Sur le serveur Windows, aller sur le Server Manager, cliquer sur `Manage` -> `Add Roles and Features` puis une fenêtre apparaît avec les étapes suivantes :  
  
➡️ **Before you begin** : cliquer sur `Next`  
  
➡️ **Installation Type** : garder `Role-based or feature-based installation` coché, puis cliquer sur `Next`  
  
➡️ **Server Selection** : on séléctionne notre serveur qui est séléctionné par défaut puis on clique sur `Next`  
  
➡️ **Select server roles** : on séléctionne `DHCP Server`, puis on clique sur `Next`   
  
➡️ **Add Roles and Features Wizard** : cliquer sur `Add Features`  
  
➡️ **Features** : on laisse par défaut, puis `Next`  
  
➡️ **DHCP Server** : `Next`  
  
➡️ **Confirmation** : `Install`  
  
➡️ Une notification apparaît en haut de la fenêtre, nous demandant de compléter la configuration du DHCP. On clique dessus puis on clique sur `Commit`, et `Close`.

# :two: Configuration de plage d'adresses IP
Le serveur DHCP a été installé avec succès sur notre Windows Server.  
  
On va maintenant passer à la configuration de la plage d'adresses IP comprises entre **172.20.0.100** et **172.20.0.200**.  
Pour ce faire :  
  
➡️ Cliquer sur `Tools` -> `DHCP`. Une fenêtre DHCP apparaît, on déroule la ligne portant le nom de notre server Windows, puis on déroule la ligne `IPv4`.  
  
➡️ On a 3 lignes supplémentaires qui apparaissent : `Server Options`, `Policies`, et `Filters`.  
  
➡️ Faire un clique droit sur IPv4 et cliquer sur `New Scope...`. Une fenêtre apparaît, cliquer sur `Next`  
  
➡️ Choisir un nom pour cette plage et cliquer sur `Next`  
  
Nous voilà sur la fenêtre qui va nous permettre de définir notre plage d'adresses IP. En face de `Start IP address`, on rentre l'IP **172.20.0.100** et en face de `End IP address`, on rentre l'IP **172.20.0.200**.  
  
➡️ On choisit un masque en /24 donc en face de `Subnet mask`, entrer **255.255.255.0** ou augmenter `Length` à **24**.  
  
➡️ Cliquer sur `Next` jusqu'à arriver à `Finish`.  
  
Maintenant, sous la ligne IPv4, on n'a plus 3 lignes mais bien 4 lignes, une ligne "Scope [172.20.0.0] name" s'étant ajoutée  
  
On a bien notre serveur DHCP avec la plage d'adresses IP configurée.  
   
Nous devons maintenant configurer l'adresse IP du serveur afin qu'il soit sur le réseau **172.20.0.0/24**.  
  
# :three: Configuration de l'adresse IP du Windows Server  
  
➡️ Ouvrir le Control Pannel -> `Network and Internet` -> `Network and Sharing Center` -> `Change adapter settings`  
  
Une fenêtre s'ouvre avec nos cartes réseau  
  
➡️ Faire un clique droit sur la carte réseau que l'on souhaite modifier, aller dans `Properties`, clique gauche sur `Internet Protocol Version 4 (TCP/IPv4)` et cliquer sur `Properties`  
  
➡️ Choisir `Use the following IP address`, en face de `IP address` entrer l'IP **172.20.0.1** et en face de `Subnet mask` entrer **255.255.255.0**  
  
➡️ Redémarrer la carte réseau, ouvrir cmd puis faire un `ipconfig`. On a bien une adresse IP en **172.20.0.1** et un masque en **255.255.255.0**  
  
# :four: Vérification de la fonctionnalité du Serveur DHCP  
  
Nous allons lancer deux clients Windows et vérifier qu'ils obtiennent bien une adresse IP faisant partie de la plage que l'on a configurée.  
  
⚠️ Le client et le serveur doivent être sur le même réseau. Dans notre cas, en utilisant des VMs, il faut aller dans les réglages réseau des trois machines et les mettre en Réseau Interne.  
  
On lance un cmd sur les deux machines client, puis on tape `ipconfig`. On a bien, pour un client, une adresse ip **172.20.0.100**, et pour le deuxième une adresse ip **172.20.0.101**.  
  
Maintenant, on va définir une adresse IP statique pour un de nos deux clients Windows lui permettant d'obtenir systématiquement l'adresse **172.20.0.10**.  
  
# 5️⃣ Mise en place d'une adresse IP statique pour un client  
  
➡️ Aller dans la fenêtre de configuration du Serveur DHCP.  
  
➡️ Dérouler la ligne `IPv4` et faire un clique droit sur `Reservations`. Cliquer sur `New Rerservation...`  
  
➡️ Choisir un nom pour cette réservation :  
  
- **IP address**: "172.20.0.10"  
  
- **MAC address** : ici on rentre l'adresse MAC de notre machine, dans notre cas, c'est "08-00-27-A4-D6-02"  
  
➡️ Fermer la fenêtre, redémarrer la carte réseau, retourner dans notre cmd et faire un `ipconfig`.  
  
La nouvelle adresse IP affichée est bien **172.20.0.10**.  
  
# 6️⃣ Vérification de la fonctionnalité de l'IP statique  
  
Nous allons vérifier que le DHCP ne fournit pas une adresse différente de **172.20.0.10** à notre client pour lequel on a configuré cette adresse par défaut en utilisant l'adresse MAC.  
Pour ce faire, ouvrir un cmd puis :  
  
➡️ Taper la commande `ipconfig /release` (cela va libérer l'adresse IP qui a été attribuée)  
  
➡️ En tapant `ipconfig`, on voit que notre client ne possède plus d'adresse IP, la commande `ipconfig /release` a donc bien fonctionné  
  
➡️ Taper la commande `ipconfig /renew` (demande d'une nouvelle adresse IP au serveur DHCP)  
  
➡️ Le serveur DHCP attribue une adresse IP au client, qui est toujours **172.20.0.10** et rien d'autre 
  
Le client qui possède la réservation n'obtient pas une autre IPv4, même si il demande un renouvellement.

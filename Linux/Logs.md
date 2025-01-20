# Quete-Les-Logs  

# 👓 Installation d'un serveur web et suivi des logs  

## :one: Installation d'Apache  
  
**Sur un serveur Debian :**  
  
➡️ Taper la commande `sudo apt update` puis `sudo apt install apache2 -y`  
  
➡️ Vérifier que le service est bien actif avec la commande `sudo systemctl status apache2`  

![image](https://github.com/user-attachments/assets/de2e949e-2fc3-4817-aa12-bdfb42b99cf9)  
  
## :two: Configuration du logging pour les accès et les erreurs  
  
Pour configurer le logging pour les accès et les erreurs, il faut modifier le fichier 000-default.conf :  
  
➡️ Dans le shell, taper `nano /etc/apache2/sites-available/000-default.conf`  
  
➡️ Décommenter les lignes encadrées en rouge sur la capture d'écran ci-dessous :  
  
![image](https://github.com/user-attachments/assets/9a14e6f4-5ef5-4203-8c01-6fcdb55f346f)  
  
➡️ Après avoir décommenté ces lignes, appliquez les modifications en redémarrant Apache :  
`sudo systemctl restart apache2`  
  
## :three: Générer du trafic sur le serveur web  
  
Afin de vérifier que les logs fonctionnent correctement, il nous faut générer du trafic sur notre serveur web. Pour ce faire, nous avons besoin de la fonctionnalité **curl**, qui permet d'envoyer des requêtes sur un serveur web, mais qui n'est pas installée par défaut sur les systèmes Debian.  
  
➡️ Télécharger **curl** : `sudo apt install curl`  
  
Une fois **curl** installé, nous pouvons désormais envoyer des requêtes vers notre serveur.  
  
➡️ `curl http:localhost`  
  
➡️ `curl http:localhost/nonexistent`  
  
➡️ `curl http:localhost/anotherpage`  
  
## :four: Analyse des logs générés  
  
Maintenant que nous avons envoyé des requêtes sur notre serveur web, nous allons analyser les logs qui ont été générés.  
  
✔️ **Identification des requêtes réussies (code 200) :**  
  
Pour afficher les logs des requêtes réussies, taper la commande suivante :  
  
`cat /var/log/apache2/acces.log | grep "200"`  
  
🚫 **Identification des requêtes non réussies (code 404) :**  
  
Pour afficher les erreurs, taper la commande suivante :  
  
`cat /var/log/apache2/access.log | grep "404"`  
  
⏲️ **Identification des adresses IP les plus fréquentes :**  
  
Pour afficher les adresses IP les plus fréquentes, taper la commande suivante :  
  
`cat /var/log/apache2/acces.log | awk '{print $1}' | sort | uniq -c | sort -nr | head`  
  
## :five: Explication de la structure des logs  
  
Pour ses logs, Apache utilise le format `combined`, comprennant **l'adresse IP**, la **date/heure**, la **requête HTTP**, le **code de réponse**, et la **taille de la réponse**.  
Nous pouvons retrouver ces informations sur la capture d'écran ci-dessous. Il s'agit du résultat de la commande  
`curl http://localhost/nonexistent` :  
  
![image](https://github.com/user-attachments/assets/98bdde04-5fcd-40c8-b448-72ffbc89dd86)  
  
🔴 : **Adresse IP** - identifie la source de la requête  
🔵 : **Date/Heure** - moment exact de la requête  
🟠 : **Requête** - type et chemin de la requête HTTP  
🟣 : **Code de réponse** - indique si la requête a réussi (200) ou échoué (404, 500...)  
🟢 : **Taille de la réponse** - quantité de données envoyées au client


  







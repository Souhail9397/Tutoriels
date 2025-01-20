Dans ce tutoriel, nous allons voir comment :  
  
:one: Créer une Unité d'Organisation "Wilders_students"  
:two: Créer un groupe d'utilisateurs "Students"  
:three: Créer un utilisateur au sein de ce groupe  

---
### :one: Créer une Unité d'Organisation "Wilders_students"  

1. Se rendre dans le Server Manager, cliquer sur "Tools" -> "Active Directory Users and Computers"
2. Une fenêtre s'affiche. Faire un clic droit sur notre domaine "wilders.lan" et cliquer sur "New" -> "Organizational Unit" et lui donner le nom "Wilders_students"
3. Notre OU "Wilders_students" est apparue dans le menu déroulant de notre domaine "wilders.lan". La création a donc réussie.

### :two: Créer un groupe d'utilisateurs "Students"  

1. Faire un clic droit sur notre OU "Wilders_students" fraîchement créée puis "New" -> "Group"
2. Nommer le groupe "Students" puis valider
3. Le groupe "Students" est bien apparu dans notre OU "Wilders_students"

### :three: Créer un utilisateur au sein de ce groupe  

1. Faire un clic droit sur "Users" dans le menu déroulant et cliquer sur "New" -> "User"
2. Entrer les informations de l'utilisateur à créer. Par exemple :
    
- First name : wilder
- Last name : test
- User login name : w.test@wilders.lan
Cliquer sur "Next". Il faut maintenant définir un mot de passe à l'utilisateur. Cliquer sur "Next" puis "Finish".  
L'utilisateur a bien été crée et est prêt à être déplacé dans le groupe "Students"

3 Faire un clic droit sur le groupe d'utilisateurs "Students" puis "Properties"
4. Se rendre dans l'onglet "Members" puis cliquer sur "Add"  
5. Entrer le nom de notre utilisateur "wilder". Cliquer sur "Ok".  
6. Cliquer sur "Ok" -> "Apply" -> "Ok"  
7. L'utilisateur a bien été ajouté au groupe "Users".w

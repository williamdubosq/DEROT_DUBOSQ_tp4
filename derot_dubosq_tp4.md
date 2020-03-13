Robin DÉROT William DUBOSQ 4ETI

# Administration Système
# TP4 - Utilisateurs, groupes et permissions

## Exercice 1. Gestion des utilisateurs et des groupes

### Question 1.
#### Commencez par créer deux groupes groupe1 et groupe2

  On crée les deux groupes à partir de la commande suivante :
  ```bash
  sudo addgroup groupe1
  sudo addgroup groupe2
  ```

### Question 2.
#### Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de leur dossier personnel et avec bash pour shell

  On crée ces utilisateurs avec la commande suivante :
  ```bash
  sudo useradd -m u1
  sudo useradd -m u2
  sudo useradd -m u3
  sudo useradd -m u4
  ```
  
  L'option -m permet de créer le dossier personnel en même temps que les utilisateurs.
  
  ### Question 3.
  #### Ajoutez les utilisateurs dans les groupes créés :
  #### - u1, u2, u4 dans groupe1
  #### - u2, u3, u4 dans groupe2
  
  On ajoute les utlisateurs dans les différents groupes avec la commande suivante :
  ```bash
  sudo usermod -a -G groupe1 u1
  sudo usermod -a -G groupe1 u2
  sudo usermod -a -G groupe1 u4
  sudo usermod -a -G groupe2 u2
  sudo usermod -a -G groupe2 u3
  sudo usermod -a -G groupe2 u4
  ```
  
  L'option -a permet de dire qu'on ajoute l'utilisateur à un groupe secondaire, et -G donne la liste des groupes secondaires auxquels il va être ajouté.
  
  ### Question 4.
  #### Donnez deux moyens d’afficher les membres de groupe2
  
  On peut utiliser la commande suivante :
  ```bash
  cat /etc/group | grep groupe1
  cat /etc/group | grep groupe1
  ```
  Ou alors :
  ```bash
  members groupe1
  members groupe2
  ```
  
  ### Question 5.
  #### Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire de /home/u3 et /home/u4

  On réalise cela à l'aide de la commande suivante :
  ```bash
  sudo chown :groupe1 /home/u1 /home/u2
  sudo chown :groupe2 /home/u3 /home/u4
  ls -l
  
  total 20
  drwxr-xr-x 8 derot derot  4096 févr. 28 16:48 derot
  drwxr-xr-x 2 u1     groupe1 4096 mars 13 13:16 u1
  drwxr-xr-x 2 u2     groupe1 4096 mars 13 13:16 u2
  drwxr-xr-x 2 u3     groupe2 4096 mars 13 13:16 u3
  drwxr-xr-x 2 u4     groupe2 4096 mars 13 13:16 u4
  ```
  
  ### Question 6.
  #### Remplacez le groupe primaire des utilisateurs :
  #### - groupe1 pour u1 et u2
  #### - groupe2 pour u3 et u4
  
  On se sert de la commande suivante :
  ```bash
  sudo usermod u1 -g groupe1
  sudo usermod u2 -g groupe1
  sudo usermod u3 -g groupe2
  sudo usermod u4 -g groupe2
  ```

  ### Question 7.
  #### Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier associé.
  
   On se sert de la commande suivante :
  ```bash
  sudo chown :groupe1 /home/groupe1
  sudo chown :groupe2 /home/groupe2
  
  sudo chmod g+w /home/groupe1
  sudo chmod g+w /home/groupe2
  ```
  
  On donne le droit au groupe d'écrire, c'est pourquoi on utilise *g+w*.
  
  ### Question 8.
  #### Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer ou supprimer ce fichier ?
  
  On veut que seul le *user* puisse écrire et donc supprimer et renommer le fichier.
  On utilise donc la commande suivante :
  ```bash
  sudo chmod g-w /home/groupe1
  sudo chmod g-w /home/groupe2
  ```
  
  ### Question 9.
  #### Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?
  
  Il n'est pas possible de se connecter en tant que u1. En effet, son mot de pase n'a pas été défini.
  Il est inactif jusqu'à ce qu'on en définisse un.
  
  ### Question 10.
  #### Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son compte.
  
  Pour mettre à jour le mot de passe de u1, on utilise la commande suivante :
  ```bash
  sudo passwd u1
  New password: u1
  Retype new password: u1
  ```
  Et pour se connecter en tant que u1 :
  ```bash
  su u1
  cd
  echo $PWD
  /home/u1
  ```
  
  #### Dans la suite du TP, les id des groupes sont décalés de 1 puisque nous avons crée un groupe test en début de TP.
  
  ### Question 11.
  #### Quels sont l’uid et le gid de u1 ?
  
  On utilise la commande suivante :
  ```bash
  id u1
  uid=1001(u1) gid=1002(groupe1) groups=1002(groupe1)
  ```
  
  ### Question 12.
  #### Quel utilisateur a pour uid 1003 ?
  
  On utilise la commande suivante :
  ```bash
  cat /etc/passwd | grep 1003
  u3:x:1003:1003::/home/u3:/bin/sh
  ```
  
  ### Question 13.
  #### Quel est l’id du groupe groupe1 ?
  
  On peut utiliser la commande *getent*:
  ```bash
  getent group groupe1
  groupe1:x:1002:u4,u1,u2
  ```
  L'id de groupe1 est donc 1002.
  
  ### Question 14.
  #### Quel groupe a pour guid 1002 ?
  
  On utilise la commande *getent*:
  ```bash
  getent group 1002
  groupe1:x:1002:u4,u1,u2
  ```
  
  ### Question 15.
  #### Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez.
  
  On peut utiliser la commande *gpasswd*. L'option -d doit permettre de supprimer un utilisateur (*delete*).
  ```bash
  sudo gpasswd -d u3 groupe2
  cat /etc/group | grep groupe2
  
  groupe2:x:1003:u2,u4
  ```
  L'utilisateur *u3* a bien été supprimé.
  
  ### Question 16.
  #### Modifiez le compte de u4 de sorte que :
  #### — il expire au 1er juin 2020
  #### — il faut changer de mot de passe avant 90 jours
  #### — il faut attendre 5 jours pour modifier un mot de passe
  #### — l’utilisateur est averti 14 jours avant l’expiration de son mot de passe
  #### — le compte sera bloqué 30 jours après expiration du mot de passe.
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  

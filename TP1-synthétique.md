
# Mise en place d'un serveur Web (très) basique
## Dans Virtualbox
VM avec :
* 1Go de RAM
* 8GO de disque, allocation dynamique
* une carte réseau connecté sur le réseau "NAT"
* une carte réseau connecté sur le réseau "réseau privé inter hôte"

## CentOS 7
* Laisser la langue par défaut à "Anglais"
* Ajouter le support du clavier Français et le mettre en haut de la piste pour qu'il soit utilisé par défaut
* Laisser les autres options par défaut, ne pas créer d'utilisateur supplémentaire

## Connexion
Commandes : ssh, w, who
* Se connecter via la console et utiliser le login root
* Se connecter en SSH via Putty (ou ssh) et utiliser le login root
* Lister les utilisateur connectés via la commande *w*


## Déplacements
Commandes : **cd**, **pwd**
Se familiariser avec les déplacements en utilisant les chemins absolus et relatifs.
Exemple, je suis positionné dans le répertoire **/tmp** et je souhaite me rendre dans **/var/lib/**.
* En chemin absolu : `cd /var/lib`
* En chemin relatif : `cd ../var/lib`


## Changer le MDP root en cas d'oubli
* Démarrer le serveur
* Une la liste des noyaux disponibles, sélectionner la première ligne puis taper "e"
* Chercher la ligne qui comme par "linux16
* Remplacer "ro" par "rw init=/sysroot/bin/sh"
* Faire Ctrl-X
```
chroot /sysroot
passwd root
touch /.autorelabel
exit
reboot
```

## Edition/manipulation de texte
Commandes : touch, vi, nano, cat, less, tail
* Créer un fichier avec touch
* Ajouter du contenu dans le fichier avec vi
* Afficher le contenu du fichier avec car, less, tail
* Installer le logiciel nano via : `yum install nano`

## Gestion du réseau, nom d'hôtes et DNS
* Vérifier la configuration de vos deux interfaces réseau
* Vérifier la configuration de votre serveur pour la résolution DNS
* Changer le nom du serveur en "web1.cesi" et rendre ce changement permanent

## Déploiement du serveur web Apache
* Installer le paquet (indice : son nom n'est pas Apache)
* Identifier les fichiers de configuration (quel est le répertoire de base pour les configurations sous Linux ?)
* Identifier du fichier de configuration "de base" (Cherchez le mot "DocumentRoot)
* Localiser du répertoire où déposer son fichier html
* Créer un fichier _users.html_ qui doit contenir les balises HTML de base et qui doit avoir pour titre "Liste des utilisateurs" et en contenu : "Liste vide"
* Vérifier l'état sur serveur Apache
* Vérifier que le serveur apache est démarré automatiquement quand votre machine démarre
* Afficher _users.html_ dans votre navigateur, que constatez-vous ?

## Configuration de la sécurité (SELinux et Firewall)
* Désactiver SElinux via `setenforce permissive`
* Rendre le mode _permissive_ persistant en éditant le fichier xxxxx
* Arrêter le service firewall


## Déploiement du serveur Mariadb
* Installer le paquet
* Vérifier l'état du service
* Se connecter au moteur MariaDB via la commande _mysql_
* Créer une base de données "app1"
* Sélectionner la base "db1"
* Ajouter une table *utilisateurs" dans la base avec la structure suivante :
```
  CREATE TABLE utilisateurs(
	    user_id INT NOT NULL AUTO_INCREMENT,
	    username VARCHAR(100) NOT NULL,
     PRIMARY KEY ( user_id )
   );
```
* Afficher le contenu de la table "utilisateurs"
* Ajouter un enregistrement dans la table "utilisateurs" avec comme "username" : "root"
* A partir de l'invite de commande bash, trouver la commande qui permet de lister le contenu de la table "utilisateurs"



# Améliorer nos pratiques de travail
Maintenant que l'on a un serveur qui opérationnel, il est grand temps d'utiliser des méthodes de travail plus sérieuses !

##Création d'un utilisateur applicatif et d'un utilisateur pour le développeur
* Créer un utilisateur portant votre nom
* Créer un répertoire "/opt/appli1"
* Créer un utilisateur _app1_ avec comme _homedir_ le répertoire de votre application web
* Rendre l'utilisateur _app1_ propriétaire de ce répertoire et permettre aux autres utilisateurs de lire et exécuter les fichiers présents dans ce répertoire
* Déplacer votre page _users.html_ dans ce nouveau répertoire et modifier la condfiguration d'Apache pour utiliser ce nouveau répertoire

## Création de règles sudo
* Autoriser votre utilisateur à devenir _root_
* Autoriser l'utilisateur _app1_ à manipuler le service Apache (status, démarrage, arrêt)

## Modifier la configuration de SSH
* Générer votre clé SSH si vous n'en avez pas
* Copier votre clé SSH sur le serveur dans le homedir de votre utilisateur
* Désactiver l'authentification par mots de passe
* Désactiver la connexion direct pour root

## Gestion du firewall
* Démarrer le service de firewall _firewalld_
* Affichier la zone par défaut
* Lister les règles pour la zone par défaut (public)
* Lister les services
* AJouter le service _http_ à la zone _public_ et rendre la règle permanente
* Recharger les règles du firewall

## Amélioration de la configuration MariaDB
**Attention** Les utilisateurs déclarés dans MariaDB n'ont aucun rapport avec ceux créés sur votre serveur. Les bases de comptes sont différentes
* Faire un paramètrage "sécurisé" de MariaDB (il y a un binaire spécifique)
* Créer un utilisateur "app1" dans MariaDB qui peut se connecter MariaDB et réaliser toutes les opérations sur la base "app1"
* Se connecter à MariaDB avec l'utilisateur "app1" et vérifiez votre identité
* Afficher les droits des utilisateurs sur la base
* (optionnel) Mettre en place une configuration permettant à l'utilisateur "appli1" de ne pas entrer de MDP quand il lance la commande mysql

##Ajouter le support de PHP pour Apache
* Installer les packgaes php
* Créer un fichier phpinfo.php dans le même répertoire que users.html, mettre le contenu :
```
   <?php
	phpinfo();
   ?>
```
* Afficher la page phpinfo.php depuis votre navigateur
* Rédémarrer le service Apache pour prendre en compte le module pour php
* Afficher la page phpinfo.php depuis votre navigateur

##Créer une page qui liste dynamiquement les utilisateurs


## Ajout d'un serveur git (optionnel)
**Attention** sur CentOS 7, il faut chercher le paquet gitolite3.
* Se baser sur le tutorial [suivant](https://https://www.digitalocean.com/community/tutorials/how-to-use-gitolite-to-control-access-to-a-git-server-on-an-ubuntu-12-04-vps pour déployer un serveur gitolite.


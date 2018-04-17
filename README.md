# ansible-roles-paquerette

*Proposition de rôles ansible **minimaux**, pour mettre en oeuvre et maintenir des applications sur des machines auto hébergées ou dans le cloud.* 

Systèmes supportés : **Ubuntu 16.04 LTS**, plus maintenu pour debian

Choix, discutables ! :

- configuration basée sur deux partitions, 1 système, 1 "production" montée sur **/mnt/vdb** par défaut
- monitoring : monit
- backup : backupninja (!), **backup externe via rsync** et "cross backup" : une machine récupère d'autres backups par grappe
- mail, remontée de messages : **postfix configuré en relai SMTP**, copie cachée systématique permettant l'envoi de sms par exemple, non détaillé ici
- tous les services web utilisent des certificats letsencrypt

**L'organisation des scripts se fait en 7 catégories :**

## 1 - L'inventaire
C'est la partie qui définit la liste des machines pilotées, la composition de la plateforme de support applicative, la liste des instances et toutes les variables utiles aux rôles.

***En gras**, les groupes à définir localement, selon les besoins.* 
- secret : les variables qui doivent rester secrètes (mot de passe etc...)
- - base_server : toutes les variables communes non secrètes
- - - **test** : toutes les variables propres aux machines de test
- - - **prod** : toutes les variables propres aux machines de production

etc...

fichiers dans group_vars
*cf hosts.example*


## 2 - Le serveur
C'est la partie qui définit les base du serveur, elle est la couche basse. Elle met en oeuvre :
- l'organisation des répertoires et fichiers
- la localisation du serveur
- la stratégie de sauvegarde
- la stratégie de monitoring

rôle : base_server

utilitaires : server_\<...\>

## 3 - La plateforme de support applicative
C'est la partie qui définit l'ensemble des services ou composants nécessaire au fonctionnement d'une instance applicative. Elle met en oeuvre :
- les serveurs web (nginx, apache,...)
- les serveurs de base de données (mariadb, postgres,...)
- les languages (php, python,...)
- le monitoring associé à ces composants

rôle : base_platform

## 4 - L'instance applicative
C'est la partie qui définit la méthode de déploiement d'une instance applicative. Elle met en oeuvre:
- le téléchargement d'une application
- la création des bases de données et dépendances (certificat letsencrypt...)
- la configuration de base de l'application
- la mise en place des sauvegardes et du monitoring
- démarrage du service

rôles : \<application\>_instance

## 5 - La mise à jour d'instance
C'est la partie qui permet de mettre à jour une instance applicative. La mise à jour d'une instance se distingue de son déploiement :
- téléchargement de la nouvelle version
- arrêt du service
- sauvegarde complète de la version courante
- mise à jour du logiciel et de la base de données + ou - automatisée selon l'application
- redémarrage du service

rôles : \<application\>_instance_upg

## 6 - Éléments réutilisables

Les rôles utilisent des parties factorisées dans des rôles réutilisables (création de base de données, de certificats etc...)

rôles : \_app\_\<fonction\>

## 7 - Gestion multi-instance (en cours de généralisation)

Il est possible d'installer directement une liste d'instance ou de piloter leur mise à jour en déclarant les instances sous forme de liste.

cf: le fichier d'inventaire host.example.yml et le rôle dolibarr

rôles : \<application\>

##

TODO : 

- mise à jour des scripts mattermost avec les éléments réutilisables
- multi instance pour nextcloud et mattermost
- fusionner les rôles php7_apache2 et php7_nginx
- failtoban pour les services
- finaliser letsencrypt sans coupure de service. (fonctionne avec coupure)

[paquerette.eu](http://paquerette.eu)
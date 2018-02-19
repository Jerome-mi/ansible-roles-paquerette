# ansible-roles-paquerette

*Proposition de rôles ansible **minimaux**, pour mettre en oeuvre et maintenir des applications sur des machines auto hébergées ou dans le cloud.* 

Systèmes supportés : **Ubuntu 16.04 LTS**, plus maintenu pour debian

Choix, discutables ! :

- configuration basée sur deux partitions, 1 système, 1 "production" montée sur **/mnt/vdb** par défaut
- monitoring : monit
- backup : backupninja (!), **backup externe via rsync** et "cross backup" : une machine récupère d'autres backups par grappe
- mail, remontée de messages : **postfix configuré en relai SMTP**, copie cachée systématique permettant l'envoi de sms par exemple, non détaillé ici
- tous les services web utilisent des certificats letsencrypt

**L'organisation des scripts se fait en 5 catégories :**

## L'inventaire
C'est la partie qui définit la liste des machines pilotées, leur composition et les variables utiles aux rôles.

***En gras**, les groupes à définir localement, selon les besoins.* 
- secret : les variables qui doivent rester secrètes (mot de passe etc...)
- - base_server : toutes les variables communes non secrètes
- - - **test** : toutes les variables propres aux machines de test
- - - **prod** : toutes les variables propres aux machines de production

etc...

fichiers dans group_vars
*cf hosts.example*


## Le serveur
C'est la partie qui définit les base du serveur, elle est la couche basse. Elle met en oeuvre :
- l'organisation des répertoires et fichiers
- la localisation du serveur
- la stratégie de sauvegarde
- la stratégie de monitoring

rôle : base_server

utilitaires : server_\<...\>

## La plateforme applicative
C'est la partie qui définit l'ensemble des services ou composants nécessaire au fonctionnement d'une instance applicative. Elle met en oeuvre :
- les serveurs web (nginx, apache,...)
- les serveurs de base de données (mariadb, postgres,...)
- les languages (php, python,...)
- le monitoring associé

rôle : base_platform

## L'instance applicative
C'est la partie qui définit la méthode de déploiement d'une instance applicative. Certaines sont multi instance, d'autres pas. Elle met en oeuvre:
- le téléchargement d'une application
- la création des bases de données et dépendances (certificat letsencrypt...)
- la configuration de base de l'application
- la mise en place des sauvegardes et du monitoring
- démarrage du service

rôles : \<application\>_instance

## La mise à jour d'instance
C'est la partie qui permet de mettre à jour une instance applicative. La mise à jour d'une instance se distingue de son déploiement :
- téléchargement de la nouvelle version
- arrêt du service
- sauvegarde de la version courante
- mise à jour du logiciel et de la base de données + ou - automatisée selon l'application
- redémarrage du service

rôles : \<application\>_upg

TODO : 

- fusionner les rôles php7_apache2 et php7_nginx
- finaliser mariadb_mysql, **sécurisation**,cohabitation possible ?
- failtoban pour les services
- finaliser letsencrypt sans coupure de service. (fonctionne avec coupure)
- étudier les listes d'instances dans les fichiers host_vars et les lancements de scripts instance et instance_upg en boucle
- améliorer les logs ansible et l'inventaire 

[paquerette.eu](http://paquerette.eu)
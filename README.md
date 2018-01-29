# ansible-roles-paquerette

*Proposition de rôles ansible **minimaux**, pour mettre en oeuvre et maintenir des applications sur des machines auto hébergées ou dans le cloud.* 

Systèmes supportés : **Ubuntu 16.04 LTS**

**REFONTE en cours : nettoyage, simplification**

**L'organisation des rôles se fait en 5 catégories :**

## L'inventaire
C'est la partie qui liste les machines pilotées et organise la définition des variables.
- secret : les variables qui doivent rester secrètes (mot de passe etc...)
- - base_server : toutes les variables communes non secrètes
- - - test : toutes les variables propres aux machines de test
- - - prod : toutes les variables propres aux machines de production

*cf hosts.example*


## Le serveur
C'est la partie qui définit les base du serveur, elle est la couche basse. Elle met en oeuvre :
- l'organisation des répertoires et fichiers
- la localisation du serveur
- la stratégie de sauvegarde
- la stratégie de monitoring

## La plateforme applicative
C'est la partie qui définit l'ensemble des services ou composants nécessaire au fonctionnement d'une instance applicative. Elle met en oeuvre :
- les serveurs web (nginx, apache,...)
- les serveurs de base de données (mariadb, postgres,...)
- les languages (php, python,...)
- le monitoring

## L'instance applicative
C'est la partie qui définit la méthode de déploiement d'une instance applicative. Certaines sont multi instance, d'autres pas. Elle met en oeuvre:
- le téléchargement d'une application
- la création des bases de données et dépendances (certificat letsencrypt...)
- la configuration de base de l'application
- la mise en place des sauvegardes et du monitoring
- démarrage du service

## La mise à jour d'instance
C'est la partie qui permet de mettre à jour une instance applicative. La mise à jour d'une instance se distingue de son déploiement :
- téléchargement de la nouvelle version
- arrêt du service
- sauvegarde de la version courante
- mise à jour du logiciel et de la base de données
- redémarrage du service

[paquerette.eu](http://paquerette.eu)
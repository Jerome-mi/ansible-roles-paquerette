# ansible-roles-paquerette

*Proposition de rôles ansible **minimaux**, pour mettre en oeuvre et maintenir des applications sur des machines auto hébergées ou dans le cloud.* 

**L'organisation des rôles se fait en 4 catégories :**

## La partie serveur
C'est la partie qui définit les base du serveur, elle est la couche basse. Elle met en oeuvre :
- l'organisation des répertoires et fichiers
- la localisation du serveur
- la stratégie de sauvegarde
- la stratégie de monitoring

## La partie plateforme applicative
C'est la partie qui définit l'ensemble des services ou composants nécessaire au fonctionnement d'une instance applicative. Elle met en oeuvre :
- les containers 
- les serveurs web (nginx, apache,...)
- les serveurs de base de données (mariadb, postgres,...)
- les languages (php, python,...) 

## La partie instance
C'est la partie qui définit la méthode de déploiement d'une instance applicative. Certaines sont multi instance, d'autres pas. Elle met en oeuvre:
- le téléchargement d'une applciation
- la création des bases de données et dépendances
- la configuration de base de l'application
- la mise en place des sauvegardes et du monitoring en respectant les couches précédentes

## La partie mise à jour
C'est la partie qui permet de mettre à jour une instance applicative.
- 

[paquerette.eu](http://paquerette.eu)
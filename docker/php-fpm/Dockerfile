#utilisation de l'image officielle PHP-FPM
FROM php:fpm-alpine
#Copier le script wait-for-it dans le conteneur
COPY wait-for-it.sh /usr/bin/wait-for-it
#Autoriser l'exécution pour l'attente
RUN chmod +x /usr/bin/wait-for-it
#Ajouter git pour l'installation des dépendances
RUN apk --update --no-cache add git
RUN apk add --no-cache bash
#copier le fichier du composer à partir de l'image officielle de Composer
COPY --from=composer /usr/bin/composer /usr/bin/composer
#Définition du répertoire de travail 
WORKDIR /var/www
#Installation des dépendances, (attendre que le conteneur database soit en ligne pour exécuter le script de migration. 
#et enfin, exécuter php-fpm pour démarrer le serveur)
CMD composer install ; wait-for-it database:5432 -- bin/console doctrine:migrations:migrate ;  php-fpm 
#Expose le port 9000 pour PHP-FPM 
EXPOSE 9000
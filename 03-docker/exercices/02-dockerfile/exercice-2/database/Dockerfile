# Utilisez une image MySQL officielle en tant qu'image de base
FROM mysql:5.7

# Copiez le fichier SQL initial dans le conteneur
COPY database.sql /docker-entrypoint-initdb.d/database.sql

# Utilisez une image Python officielle en tant qu'image de base
FROM python:3.8

# Définissez le répertoire de travail
WORKDIR /app

# Copiez les fichiers nécessaires dans le conteneur
COPY server.py .
COPY requirements.txt .

# Installez les dépendances Python
RUN pip install -r requirements.txt

# Copiez le contenu du répertoire actuel dans le conteneur
COPY . .

# Exposez le port sur lequel l'application écoute
EXPOSE 80

# Commande pour démarrer l'application
CMD ["python", "server.py"]

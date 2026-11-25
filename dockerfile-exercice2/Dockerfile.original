# Image de base Node.js légère
FROM node:18-alpine

# Répertoire de travail de l'application
WORKDIR /app

# Copier les fichiers de dépendances, puis les installer
COPY package.json package-lock.json ./
RUN npm install

# Copier tout le code de l'application
COPY . .

# Construire l'application (résultat dans /app/dist) et installer le serveur web
RUN npm run build && npm install -g serve

# Port sur lequel serve écoute
EXPOSE 3000

# Démarrer l'application en servant le dossier dist
CMD ["serve", "-s", "dist"]
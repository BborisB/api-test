# Étape 1 : Build de l'application
FROM node:22.13.1-alpine3.21 AS builder

# Définir le répertoire de travail
WORKDIR /usr/src/app

# Copier uniquement les fichiers nécessaires pour installer les dépendances
COPY package*.json ./

# Installer les dépendances de production uniquement
RUN npm install --only=production

# Copier le code source
COPY . .

# Construire l'application Nest.js
RUN npm run build

# Étape 2 : Créer une image finale et optimisée
FROM node:22.13.1-alpine3.21

# Définir le répertoire de travail
WORKDIR /usr/src/app

# Copier uniquement les fichiers nécessaires depuis l'étape de build
COPY --from=builder /usr/src/app/node_modules ./node_modules
COPY --from=builder /usr/src/app/dist ./dist
COPY --from=builder /usr/src/app/package*.json ./

# Exposer le port sur lequel tourne l'API
EXPOSE 3000

# Commande pour démarrer l'application
CMD ["node", "dist/main.js"]

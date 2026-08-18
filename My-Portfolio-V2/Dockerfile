# Étape 1 : Build de l'application avec Node.js
FROM node:20-alpine AS builder

WORKDIR /app

# Copie des fichiers de dépendances
COPY package*.json ./
RUN npm install

# Copie du reste du code source
COPY . .

# Déclaration des arguments de build pour les variables d'environnement
ARG VITE_WEB3FORMS_ACCESS_KEY=5845ec3c-0cab-4d15-9c85-8a52b4fbcc84
ARG VITE_WEB3FORMS_URL=https://api.web3forms.com/submit

# Transmission aux variables d'environnement du process de build
ENV VITE_WEB3FORMS_ACCESS_KEY=$VITE_WEB3FORMS_ACCESS_KEY
ENV VITE_WEB3FORMS_URL=$VITE_WEB3FORMS_URL

# Build de l'application (Vite génère le dossier /dist)
RUN npm run build

# Étape 2 : Serveur Web Nginx
FROM nginx:alpine

# Copie du build Vite vers le dossier HTML par défaut de Nginx
COPY --from=builder /app/dist /usr/share/nginx/html

# Copie d'une configuration Nginx minimale pour gérer le routage Single Page App (SPA)
RUN echo 'server { \
    listen 80; \
    location / { \
        root /usr/share/nginx/html; \
        index index.html index.htm; \
        try_files $uri $uri/ /index.html; \
    } \
}' > /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]

# Etapa 1: Construcción del proyecto
FROM node:18-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Etapa 2: Servidor web de producción robusto
FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html

# Inyectamos una configuración limpia para Nginx que soporte SPA y assets correctos
RUN echo 'server { \
    listen 80; \
    server_name localhost; \
    location / { \
        root /usr/share/nginx/html; \
        index index.html index.htm; \
        try_files $uri $uri/ /index.html; \
    } \
}' > /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

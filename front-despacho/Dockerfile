# Etapa 1: Construcción del proyecto
FROM node:18-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Etapa 2: Servidor web de producción con Proxy Inverso
FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html

# Configuración de Nginx para redirigir el tráfico al Backend de K8s
RUN echo 'server { \
    listen 80; \
    server_name localhost; \
    \
    location / { \
        root /usr/share/nginx/html; \
        index index.html index.htm; \
        try_files $uri $uri/ /index.html; \
    } \
    \
    # Redirigir peticiones de ventas al backend correspondiente \
    location /ventas { \
        proxy_pass http://back-ventas-service:8080; \
        proxy_set_header Host $host; \
        proxy_set_header X-Real-IP $remote_addr; \
    } \
    \
    # Redirigir peticiones de despachos al backend correspondiente \
    location /despachos { \
        proxy_pass http://back-despacho-service:8081; \
        proxy_set_header Host $host; \
        proxy_set_header X-Real-IP $remote_addr; \
    } \
}' > /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

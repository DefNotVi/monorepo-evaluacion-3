FROM nginx:alpine
# Copia todo el contenido del front al directorio público de Nginx
COPY . /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

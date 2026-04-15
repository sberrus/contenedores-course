# Resumen de Dockerfile

## ¿Qué es un Dockerfile?
Un Dockerfile es un archivo de texto que contiene instrucciones para construir una imagen Docker de forma automatizada.

---

## Estructura básica

```
FROM node:18
WORKDIR /app
COPY . .
RUN npm install
CMD ["npm","start"]
```

---

## Directivas principales

### FROM
Define la imagen base.
```
FROM ubuntu:22.04
```

### WORKDIR
Define el directorio de trabajo dentro del contenedor.
```
WORKDIR /app
```

### COPY
Copia archivos desde el host al contenedor.
```
COPY . .
```

### ADD
Similar a COPY, pero permite URLs y descompresión automática.

### RUN
Ejecuta comandos durante la construcción.
```
RUN apt update && apt install -y curl
```

### CMD
Define el comando por defecto al ejecutar el contenedor.
```
CMD ["node","app.js"]
```

### ENTRYPOINT
Define el punto de entrada principal.
```
ENTRYPOINT ["python"]
```

### ENV
Define variables de entorno.
```
ENV NODE_ENV=production
```

### EXPOSE
Indica el puerto que usa la aplicación.
```
EXPOSE 3000
```

### VOLUME
Define un volumen.
```
VOLUME /data
```

---

## Buenas prácticas

- Usar imágenes ligeras (alpine, slim)
- Minimizar capas combinando RUN
- Usar .dockerignore
- Aprovechar la cache (copiar package.json antes)
- No usar root (USER)

---

## Multi-stage builds

Permite reducir el tamaño final:

```
FROM node:18 AS builder
WORKDIR /app
COPY . .
RUN npm install && npm run build

FROM nginx:alpine
COPY --from=builder /app/build /usr/share/nginx/html
```

## Dockerfile con buenas prácicas de ciberseguridad
### 1. Imagen base ligera y específica (evitar latest) estabilidad en las versiones.
FROM node:18-alpine

### 2. Crear usuario no root
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

### 3. Definir directorio de trabajo
WORKDIR /app

### 4. Copiar solo archivos necesarios primero (mejor cache)
COPY package*.json ./

### 5. Instalar dependencias (sin devDependencies en producción)
RUN npm ci --only=production

### 6. Copiar el resto del código
COPY . .

### 7. Cambiar propietario de archivos
RUN chown -R appuser:appgroup /app

### 8. Usar usuario no privilegiado
USER appuser

### 9. Exponer puerto (documentativo)
EXPOSE 3000

### 10. Comando de arranque seguro
CMD ["node", "server.js"]
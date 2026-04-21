# 1. ¿Qué son las imágenes en Docker?

Una **imagen** es una **plantilla inmutable** que contiene todo lo necesario para ejecutar una aplicación:

* Código fuente
* Librerías
* Dependencias
* Variables de entorno
* Configuración

👉 A partir de una imagen se crean los **contenedores**.

📌 Idea clave:

> Imagen = plano (template)
> Contenedor = instancia en ejecución

---

# 2. ¿Cómo se gestionan las imágenes?

## Comandos básicos

### Descargar imagen

```bash
docker pull node
```

### Listar imágenes

```bash
docker images
```

### Eliminar imagen

```bash
docker rmi node
```

### Construir imagen desde Dockerfile

```bash
docker build -t mi-app .
```

### Etiquetar imagen

```bash
docker tag mi-app usuario/mi-app:1.0
```

### Subir imagen a un registry

```bash
docker push usuario/mi-app:1.0
```

---

## Conceptos importantes

* 🔹 Las imágenes están formadas por **capas (layers)**.
* 🔹 Cada instrucción en el Dockerfile crea una capa.
* 🔹 Las capas se cachean → builds más rápidos.
* 🔹 Son **inmutables** (no se modifican, se reconstruyen).

---

# 3. ¿Qué es un Dockerfile?

Un **Dockerfile** es un archivo de texto con instrucciones para construir una imagen automáticamente.

---

## Instrucciones más importantes

* **FROM** → imagen base
* **WORKDIR** → directorio de trabajo
* **COPY / ADD** → copiar archivos
* **RUN** → ejecutar comandos
* **CMD** → comando por defecto
* **ENTRYPOINT** → comando principal
* **EXPOSE** → documenta puertos
* **ENV** → variables de entorno

---

## Características clave

* 🔹 Automatiza la creación de imágenes
* 🔹 Permite builds reproducibles
* 🔹 Usa cache para optimizar tiempos
* 🔹 Define entornos completos
* 🔹 Se integra con CI/CD

---

# 4. Ejemplos básicos de Dockerfile

## Ejemplo 1: Node.js simple

```dockerfile
FROM node:18

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

---

## Ejemplo 2: Python

```dockerfile
FROM python:3.11

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```

---

## Ejemplo 3: Nginx con HTML estático

```dockerfile
FROM nginx:alpine

COPY ./html /usr/share/nginx/html
```

---

# 5. ¿Qué son los multistage builds?

Los **multistage builds** permiten usar **varias etapas (stages)** dentro de un mismo Dockerfile.

👉 Se usan para:

* Separar build y producción
* Reducir tamaño de la imagen final
* Mejorar seguridad
* Eliminar dependencias innecesarias

📌 Idea clave:

> Solo copias al final lo necesario para ejecutar la app

---

# 6. Ejemplo básico de multistage

```dockerfile
# Etapa 1: build
FROM node:18 AS builder

WORKDIR /app
COPY . .
RUN npm install
RUN npm run build

# Etapa 2: producción
FROM nginx:alpine

COPY --from=builder /app/build /usr/share/nginx/html
```

---

# 7. Ejemplo real: Next.js (build + producción)

Este es muy típico en proyectos modernos 👇

```dockerfile
# ---------- STAGE 1: BUILD ----------
FROM node:18 AS builder

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

# Build de Next.js
RUN npm run build


# ---------- STAGE 2: PRODUCCIÓN ----------
FROM node:18-alpine

WORKDIR /app

# Solo copiamos lo necesario
COPY --from=builder /app/package*.json ./
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
COPY --from=builder /app/node_modules ./node_modules

EXPOSE 3000

CMD ["npm", "start"]
```

---

## ¿Qué está pasando aquí?

### Stage 1 (builder)

* Instala dependencias
* Compila la app (`next build`)

### Stage 2 (producción)

* Usa una imagen más ligera (`alpine`)
* Solo copia lo necesario
* No incluye herramientas de desarrollo

---

## Ventajas de este enfoque

* 🔹 Imagen final mucho más ligera
* 🔹 Mayor seguridad
* 🔹 Arranque más rápido
* 🔹 Separación clara build/runtime

---

# 8. Ideas clave para examen 🚀

* 🔹 Una imagen es **inmutable y basada en capas**
* 🔹 Dockerfile = receta para construir imágenes
* 🔹 Cada instrucción crea una capa
* 🔹 Multistage reduce tamaño y mejora eficiencia
* 🔹 `COPY --from=builder` es clave en multistage
* 🔹 Siempre usar imágenes ligeras en producción (`alpine`)

---

Si quieres, en el siguiente paso puedo darte:

* 🧠 **preguntas tipo examen**
* ⚠️ **errores comunes con Dockerfile**
* 🔧 o cómo optimizar imágenes (best practices pro)

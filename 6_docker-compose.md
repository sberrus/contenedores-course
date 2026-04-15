# 1. ¿Qué es Docker Compose?

**Docker Compose** es una herramienta que permite **definir y ejecutar aplicaciones multicontenedor** usando un archivo YAML (`docker-compose.yml`).

👉 En lugar de lanzar contenedores uno a uno, defines todo en un solo archivo y lo levantas con un comando.

```bash
docker compose up
```

---

# 2. Características principales

* 🔹 Define múltiples servicios en un solo archivo
* 🔹 Permite levantar toda la arquitectura con un comando
* 🔹 Gestiona redes automáticamente
* 🔹 Facilita la comunicación entre contenedores
* 🔹 Permite definir volúmenes (persistencia)
* 🔹 Soporta variables de entorno
* 🔹 Permite builds automáticos
* 🔹 Escalado de servicios (`--scale`)
* 🔹 Ideal para desarrollo y testing

---

# 3. Estructura básica de un docker-compose.yml

```yaml
version: '3.9'

services:
  web:
    image: nginx
    ports:
      - "8080:80"

  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: example
```

---

# 4. Ejemplo básico completo

```yaml
version: '3.9'

services:
  app:
    image: node:18
    container_name: mi-app
    ports:
      - "3000:3000"
    volumes:
      - .:/app
    working_dir: /app
    command: npm start

  db:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: secret
    volumes:
      - db-data:/var/lib/mysql

volumes:
  db-data:
```

---

# 5. ¿Cómo se comunican los contenedores?

👉 Docker Compose crea automáticamente una **red interna**.

* Cada servicio puede acceder a otro usando su **nombre de servicio como hostname**

Ejemplo:

```bash
http://db:3306
```

📌 No necesitas IPs → Docker incluye DNS interno.

---

# 6. ¿Cómo funciona la red en Docker Compose?

* 🔹 Se crea una red por defecto (tipo bridge)
* 🔹 Todos los servicios se conectan automáticamente
* 🔹 Puedes definir redes personalizadas

Ejemplo:

```yaml
networks:
  mi-red:
```

```yaml
services:
  app:
    networks:
      - mi-red
```

---

# 7. Persistencia de datos

Se hace con **volúmenes**:

```yaml
services:
  db:
    image: mysql
    volumes:
      - db-data:/var/lib/mysql

volumes:
  db-data:
```

👉 Los datos sobreviven aunque el contenedor se elimine.

---

# 8. Ejemplo tipo producción (web + backend + db + proxy)

```yaml
version: '3.9'

services:
  frontend:
    image: nginx
    ports:
      - "80:80"

  backend:
    image: mi-backend:latest
    environment:
      DB_HOST: db

  db:
    image: postgres:15
    volumes:
      - postgres-data:/var/lib/postgresql/data

  redis:
    image: redis

volumes:
  postgres-data:
```

---

# 9. Ejemplo con características más avanzadas

```yaml
version: '3.9'

services:
  app:
    image: node:18
    depends_on:
      - db
    environment:
      DB_HOST: db
    restart: always

  db:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: secret
    volumes:
      - db-data:/var/lib/mysql
    restart: always

volumes:
  db-data:
```

👉 Aquí aparecen cosas importantes:

* `depends_on` → orden de arranque
* `restart` → reinicio automático

---

# 10. Ejemplo con build incluido

```yaml
version: '3.9'

services:
  app:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - .:/app
    command: npm run dev
```

👉 Docker Compose construye la imagen automáticamente.

---

## Build más avanzado

```yaml
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile.prod
```

---

# 11. Comandos importantes

```bash
docker compose up          # levantar servicios
docker compose up -d       # en segundo plano
docker compose down        # parar y eliminar
docker compose ps          # ver contenedores
docker compose logs        # logs
docker compose build       # construir imágenes
```

---

# 12. Conceptos clave para examen 🚀

* 🔹 Compose = gestión de múltiples contenedores
* 🔹 Archivo YAML define toda la arquitectura
* 🔹 Servicios se comunican por nombre (DNS interno)
* 🔹 Red automática
* 🔹 Volúmenes = persistencia
* 🔹 `depends_on` no espera a que el servicio esté listo (⚠️ típico error)
* 🔹 `build` permite crear imágenes desde Dockerfile
* 🔹 Ideal para microservicios

---

# 13. Cosas importantes (nivel práctico/pro)

### ⚠️ 1. depends_on NO espera readiness

Necesitas herramientas como:

* wait-for-it
* healthchecks

---

### 🔹 2. Healthcheck

```yaml
services:
  db:
    image: postgres
    healthcheck:
      test: ["CMD", "pg_isready"]
```

---

### 🔹 3. Variables de entorno (.env)

```env
DB_PASSWORD=secret
```

```yaml
environment:
  - DB_PASSWORD=${DB_PASSWORD}
```

---

### 🔹 4. Escalado

```bash
docker compose up --scale app=3
```

---

### 🔹 5. Override files

```bash
docker-compose.override.yml
```

👉 Muy usado para separar dev/prod

---

# 14. Ejemplo realista (tipo proyecto moderno)

```yaml
version: '3.9'

services:
  nextjs:
    build: .
    ports:
      - "3000:3000"
    environment:
      API_URL: http://api:4000
    depends_on:
      - api

  api:
    image: node:18
    command: node server.js

  db:
    image: mongo
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:
```

---

# 15. Resumen rápido tipo chuleta 🧠

* Compose = múltiples contenedores
* YAML → define servicios
* Red automática → comunicación por nombre
* Volúmenes → persistencia
* `build` → crea imágenes
* `depends_on` → orden (no readiness)
* `up / down` → levantar/parar todo

---

Si quieres, puedo darte:

* 🧠 **ejercicios prácticos tipo examen**
* ⚠️ **errores típicos en Compose**
* 🔧 o un **proyecto completo paso a paso (frontend + backend + db)**

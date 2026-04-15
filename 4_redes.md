## 1. ¿Qué son las redes en Docker?

Las **redes en Docker** permiten que los contenedores:

* Se comuniquen entre sí
* Se comuniquen con el exterior (internet)
* Aíslen el tráfico según necesidades

👉 Son clave en aplicaciones con varios servicios (por ejemplo: backend + base de datos).

---

## 2. Tipos de redes en Docker

Docker incluye varios tipos de red por defecto:

### 1. bridge (por defecto)

* Es la red que usa Docker si no especificas otra.
* Permite comunicación entre contenedores en el mismo host.
* Cada contenedor tiene una IP interna.

```bash
docker run nginx
```

👉 Problema: hay que usar IPs o configurar nombres manualmente.

---

### 2. host

* El contenedor comparte la red del host.
* No hay aislamiento de red.

```bash
docker run --network host nginx
```

👉 Más rápido, pero menos seguro.

---

### 3. none

* El contenedor no tiene red.

```bash
docker run --network none nginx
```

👉 Total aislamiento.

---

### 4. redes personalizadas (user-defined bridge)

* Recomendadas en la práctica.
* Permiten que los contenedores se comuniquen por **nombre**.

```bash
docker network create mi-red
docker run -d --name web --network mi-red nginx
docker run -d --name db --network mi-red mysql
```

👉 Aquí `web` puede comunicarse con `db` usando el nombre `db`.

---

### 5. overlay (avanzado)

* Para comunicación entre contenedores en distintos hosts.
* Usado en Docker Swarm o entornos distribuidos.

---

## 3. Comandos básicos de redes

### Listar redes

```bash
docker network ls
```

### Crear red

```bash
docker network create mi-red
```

### Inspeccionar red

```bash
docker network inspect mi-red
```

### Eliminar red

```bash
docker network rm mi-red
```

---

## 4. Conectar contenedores a redes

### Ejecutar contenedor en una red

```bash
docker run -d --network mi-red nginx
```

### Conectar un contenedor existente

```bash
docker network connect mi-red mi-contenedor
```

### Desconectar contenedor

```bash
docker network disconnect mi-red mi-contenedor
```

---

## 5. Puertos y acceso externo

Para acceder desde fuera del contenedor:

```bash
docker run -p 8080:80 nginx
```

👉 Esto significa:

* Puerto **8080 del host**
* Redirige al puerto **80 del contenedor**

---

## 6. Información importante (muy típica de examen)

* 🔹 Cada contenedor tiene su propia IP interna.
* 🔹 En redes personalizadas, los contenedores se comunican por **nombre** (DNS interno).
* 🔹 La red `bridge` por defecto es limitada → mejor usar redes personalizadas.
* 🔹 `host` elimina aislamiento de red.
* 🔹 `none` = sin red.
* 🔹 Los puertos deben exponerse con `-p` para acceso externo.
* 🔹 Docker incluye un sistema de **DNS interno automático**.

---

## 7. Ejemplo práctico típico

```bash
docker network create app-net

docker run -d --name backend --network app-net nginx
docker run -d --name frontend --network app-net nginx
```

👉 `frontend` puede hacer peticiones a:

```
http://backend
```

---

Si quieres, en el siguiente paso puedo darte:

* 🧠 **resumen ultra corto tipo chuleta**
* ⚠️ **errores típicos en redes Docker**
* 🔧 o cómo funcionan las redes en **Docker Compose** (muy importante en proyectos reales)

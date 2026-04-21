## 1. ¿Qué son los volúmenes en Docker?

Los **volúmenes** son mecanismos de almacenamiento que permiten **persistir datos fuera del contenedor**.

Esto significa que:

* Los datos **no se pierden** aunque el contenedor se elimine.
* Se pueden **compartir datos entre contenedores**.
* Están gestionados por Docker (no dependen directamente del sistema de archivos del host).

👉 Son fundamentales para bases de datos, logs o cualquier dato que deba mantenerse.

---

## 2. Tipos de almacenamiento en Docker

Docker ofrece principalmente **3 tipos**:

### 1. Volúmenes (Volumes) → *recomendado*

* Gestionados por Docker.
* Se almacenan en una ruta interna (ej: `/var/lib/docker/volumes/`).
* Más seguros y portables.

Ejemplo:

```bash
docker volume create mi-volumen
docker run -v mi-volumen:/data nginx
```

---

### 2. Bind Mounts

* Enlazan una carpeta del host con el contenedor.
* Tienes control total sobre los archivos desde tu sistema.

Ejemplo:

```bash
docker run -v /home/usuario/data:/data nginx
```

⚠️ Dependen de la estructura del sistema anfitrión (menos portables).

---

### 3. tmpfs mounts

* Se almacenan en memoria (RAM).
* Los datos se pierden al detener el contenedor.

Ejemplo:

```bash
docker run --tmpfs /data nginx
```

👉 Útiles para datos temporales o sensibles.

---

## 3. Comandos para gestionar volúmenes

### Crear un volumen

```bash
docker volume create mi-volumen
```

### Listar volúmenes

```bash
docker volume ls
```

### Inspeccionar un volumen

```bash
docker volume inspect mi-volumen
```

### Eliminar un volumen

```bash
docker volume rm mi-volumen
```

### Eliminar volúmenes no usados

```bash
docker volume prune
```

---

## 4. Cómo ver información y uso de volúmenes

* Ver detalles completos:

```bash
docker volume inspect mi-volumen
```

* Ver volúmenes asociados a un contenedor:

```bash
docker inspect mi-contenedor
```

* Ver dónde se almacenan físicamente:

  * Normalmente en:

  ```
  /var/lib/docker/volumes/
  ```

---

## 5. Información relevante (muy importante para examen)

* 🔹 Los volúmenes **persisten aunque el contenedor se elimine**.
* 🔹 Se pueden **compartir entre múltiples contenedores**.
* 🔹 Son la forma recomendada frente a bind mounts en producción.
* 🔹 Mejoran el rendimiento frente a escribir directamente en el contenedor.
* 🔹 Permiten hacer **backups y migraciones fácilmente**.
* 🔹 Evitan modificar la capa de escritura del contenedor.

---

## 6. Ejemplo práctico típico

```bash
docker run -d \
  --name mysql \
  -v datos-mysql:/var/lib/mysql \
  mysql
```

👉 Aquí:

* `datos-mysql` guarda la base de datos
* Si el contenedor se elimina, los datos siguen existiendo

---

Si quieres, en el siguiente paso puedo darte:

* 🧠 **preguntas tipo examen**
* ⚠️ **errores comunes con volúmenes**
* 🔧 o cómo usar volúmenes con **Docker Compose** (muy importante en cursos)

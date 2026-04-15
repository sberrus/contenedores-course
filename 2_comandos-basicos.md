## 1. Gestión de imágenes

* **Listar imágenes**

  ```bash
  docker images
  ```

* **Descargar una imagen**

  ```bash
  docker pull nginx
  ```

* **Eliminar una imagen**

  ```bash
  docker rmi nginx
  ```

* **Construir una imagen desde un Dockerfile**

  ```bash
  docker build -t mi-imagen .
  ```

---

## 2. Gestión de contenedores

* **Crear y ejecutar un contenedor**

  ```bash
  docker run nginx
  ```

* **Ejecutar en segundo plano (detached)**

  ```bash
  docker run -d nginx
  ```

* **Asignar nombre a un contenedor**

  ```bash
  docker run -d --name mi-contenedor nginx
  ```

* **Listar contenedores en ejecución**

  ```bash
  docker ps
  ```

* **Listar todos los contenedores (incluidos detenidos)**

  ```bash
  docker ps -a
  ```

* **Detener un contenedor**

  ```bash
  docker stop mi-contenedor
  ```

* **Iniciar un contenedor detenido**

  ```bash
  docker start mi-contenedor
  ```

* **Reiniciar un contenedor**

  ```bash
  docker restart mi-contenedor
  ```

* **Eliminar un contenedor**

  ```bash
  docker rm mi-contenedor
  ```

---

## 3. Inspección y logs

* **Ver logs de un contenedor**

  ```bash
  docker logs mi-contenedor
  ```

* **Inspeccionar detalles de un contenedor**

  ```bash
  docker inspect mi-contenedor
  ```

* **Ver procesos dentro del contenedor**

  ```bash
  docker top mi-contenedor
  ```

---

## 4. Interacción con contenedores

* **Acceder a un contenedor en ejecución**

  ```bash
  docker exec -it mi-contenedor bash
  ```

* **Ejecutar un comando dentro del contenedor**

  ```bash
  docker exec mi-contenedor ls
  ```

---

## 5. Volúmenes

* **Crear un volumen**

  ```bash
  docker volume create mi-volumen
  ```

* **Listar volúmenes**

  ```bash
  docker volume ls
  ```

* **Usar un volumen**

  ```bash
  docker run -v mi-volumen:/data nginx
  ```

---

## 6. Redes

* **Listar redes**

  ```bash
  docker network ls
  ```

* **Crear una red**

  ```bash
  docker network create mi-red
  ```

* **Ejecutar contenedor en una red**

  ```bash
  docker run --network mi-red nginx
  ```

---

## 7. Limpieza

* **Eliminar contenedores detenidos**

  ```bash
  docker container prune
  ```

* **Eliminar imágenes no usadas**

  ```bash
  docker image prune
  ```

* **Limpiar todo (contenedores, imágenes, redes no usadas)**

  ```bash
  docker system prune
  ```

---

Si quieres, puedo darte una versión **ultra resumida tipo chuleta de examen** o añadir **ejemplos prácticos paso a paso** (muy típico en cursos) 👍

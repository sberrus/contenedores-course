## 1. Docker: conceptos principales a tener en cuenta

**Docker** es una plataforma que permite crear, distribuir y ejecutar aplicaciones dentro de contenedores.

Conceptos clave:

* **Imagen (Image)**
  Plantilla inmutable que contiene todo lo necesario para ejecutar una aplicación (código, librerías, dependencias, etc.).

* **Contenedor (Container)**
  Instancia en ejecución de una imagen. Es ligero, aislado y portátil.

* **Dockerfile**
  Archivo de texto con instrucciones para construir una imagen paso a paso.

* **Docker Hub / Registry**
  Repositorio donde se almacenan imágenes (públicas o privadas).

* **Volúmenes (Volumes)**
  Mecanismo para persistir datos fuera del contenedor.

* **Redes (Networks)**
  Permiten la comunicación entre contenedores.

* **Docker Engine**
  Motor que ejecuta y gestiona los contenedores.

---

## 2. ¿Qué es un contenedor?

Un **contenedor** es una unidad ligera, portátil y aislada que incluye todo lo necesario para ejecutar una aplicación:

* Código
* Runtime
* Librerías
* Dependencias
* Configuración

Características principales:

* **Aislamiento**: cada contenedor funciona de forma independiente.
* **Portabilidad**: funciona igual en cualquier entorno (desarrollo, testing, producción).
* **Ligereza**: comparte el kernel del sistema operativo, por lo que consume menos recursos que una VM.
* **Arranque rápido**: se inicia en segundos.

---

## 3. Principales diferencias entre un contenedor y una VM

| Característica      | Contenedor                   | Máquina Virtual (VM)       |
| ------------------- | ---------------------------- | -------------------------- |
| Virtualización      | A nivel de sistema operativo | A nivel de hardware        |
| Sistema operativo   | Comparte el host             | Cada VM tiene su propio SO |
| Peso                | Ligero                       | Pesado                     |
| Tiempo de arranque  | Segundos                     | Minutos                    |
| Consumo de recursos | Bajo                         | Alto                       |
| Portabilidad        | Alta                         | Media                      |
| Aislamiento         | Menor que VM                 | Mayor                      |

---

## 4. Principales problemas que busca solucionar Docker

Docker surge para resolver varios problemas comunes en el desarrollo y despliegue de software:

* **"En mi máquina funciona"**
  Evita inconsistencias entre entornos (desarrollo vs producción).

* **Gestión de dependencias**
  Cada contenedor incluye sus propias dependencias, evitando conflictos.

* **Despliegues complejos**
  Simplifica la distribución de aplicaciones mediante imágenes.

* **Escalabilidad**
  Permite replicar contenedores fácilmente.

* **Uso ineficiente de recursos (en VMs)**
  Reduce el consumo al no requerir múltiples sistemas operativos completos.

* **Tiempo de configuración**
  Automatiza la creación de entornos mediante Dockerfiles.

---

Si quieres, en el siguiente paso puedo convertir esto en **apuntes tipo examen**, **resumen aún más corto**, o añadir **comandos básicos de Docker** 👍

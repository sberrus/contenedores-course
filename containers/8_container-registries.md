# Container registries
Los Container registries son repositorios donde nosotros podemos almacenar las imágenes que hayamos creado en local y luego pushearlas a algún repo para poder consumirlas luego en otros sistemas.

El más popular y el configurado por defecto es **Docker Hub** `hub.docker.com`, pero cada proveedor de cloud por lo general tienen su propio gestor de repositorios de imagenes. Por ejemplo:
- Microsoft (Azure container registry, Microsoft Container Registry).
- Amazon Elastic Container Registry.
- Google Container Registry.

En este caso usaremos dockerhub para este caso.

## Comandos básicos
Antes que nada se debe registrar el usuario en docker hub antes de hacer los siguientes pasos. 

Para logearse tenemos el siguiente comando que te guía en el proceso de login o puedes pasar las credenciales manualmente mediante tags.

- `docker login`: nos permite logearnos en docker-hub. Tiene las siguientes flags:
  - -u <usuario>: para definir un nombre de usuario.
  - -p <password>: para definir la contraseña de las sesíon.

### Gestión de imagenes

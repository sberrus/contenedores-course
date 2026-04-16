# 🔐 1. Principios básicos de seguridad en contenedores

Antes de entrar en detalles:

* 🔹 **Principio de mínimo privilegio**
* 🔹 **Reducir superficie de ataque**
* 🔹 **Aislamiento**
* 🔹 **No confiar en imágenes externas**
* 🔹 **Defensa en profundidad** (no depender de una sola capa)

---

# 🧱 2. Seguridad en imágenes Docker

## ⚠️ Riesgos principales

* Imágenes con malware o backdoors
* Dependencias vulnerables
* Imágenes demasiado grandes (más superficie de ataque)

---

## ✅ Buenas prácticas

### 1. Usar imágenes oficiales o verificadas

* Preferir:

  * `node:alpine`
  * `nginx:alpine`

👉 Evitar imágenes desconocidas de Docker Hub.

---

### 2. Usar imágenes ligeras

* `alpine`, `distroless`

✔️ Menos paquetes → menos vulnerabilidades

---

### 3. Escanear imágenes

Herramientas:

* Docker Scout
* Trivy
* Snyk

---

### 4. Fijar versiones (NO usar latest)

❌ Malo:

```bash
docker pull node:latest
```

✅ Bueno:

```bash
docker pull node:18.19-alpine
```

---

### 5. Eliminar herramientas innecesarias

Ejemplo:

```dockerfile
RUN apt-get update && apt-get install -y curl \
 && rm -rf /var/lib/apt/lists/*
```

---

### 6. No incluir secretos en la imagen

❌ Nunca:

* passwords
* tokens
* API keys

---

# 🐳 3. Seguridad en contenedores

## ⚠️ Problemas comunes

* Contenedores corriendo como root
* Demasiados permisos
* Acceso innecesario a recursos del host

---

## ✅ Buenas prácticas

### 1. No ejecutar como root

```dockerfile
RUN useradd -m appuser
USER appuser
```

---

### 2. Limitar recursos

```bash
docker run --memory="512m" --cpus="1.0" nginx
```

---

### 3. Sistema de archivos en solo lectura

```bash
docker run --read-only nginx
```

---

### 4. Limitar capacidades del kernel

```bash
docker run --cap-drop ALL nginx
```

👉 Solo añadir lo necesario.

---

### 5. Evitar modo privilegiado

❌ Nunca:

```bash
docker run --privileged
```

---

### 6. Controlar logs

* No exponer datos sensibles
* Rotar logs

---

# 🌐 4. Seguridad en redes Docker

## ⚠️ Riesgos

* Exposición innecesaria de puertos
* Comunicación no controlada entre contenedores

---

## ✅ Buenas prácticas

### 1. Exponer solo lo necesario

```bash
-p 127.0.0.1:3000:3000
```

👉 Solo accesible localmente

---

### 2. Usar redes separadas

* frontend
* backend
* db

---

### 3. No exponer bases de datos

❌ Evitar:

```bash
-p 3306:3306
```

---

### 4. Usar firewalls

* iptables
* ufw

---

# 💾 5. Seguridad en volúmenes

## ⚠️ Riesgos

* Acceso a archivos sensibles del host
* Persistencia de datos comprometidos

---

## ✅ Buenas prácticas

### 1. Evitar bind mounts innecesarios

❌

```bash
-v /:/host
```

---

### 2. Controlar permisos

* Solo lectura cuando sea posible:

```bash
-v datos:/data:ro
```

---

### 3. Separar datos críticos

* Bases de datos en volúmenes dedicados

---

# ⚙️ 6. Seguridad en Docker Compose

## ✅ Buenas prácticas

### 1. Variables de entorno seguras

❌ No hardcodear:

```yaml
environment:
  PASSWORD: 123456
```

✅ Usar `.env` o secretos

---

### 2. Usar `secrets` (en producción)

```yaml
secrets:
  db_password:
    file: ./db_password.txt
```

---

### 3. Limitar servicios expuestos

* Solo frontend expuesto
* backend y db internos

---

### 4. Reinicio controlado

```yaml
restart: unless-stopped
```

---

### 5. Definir usuarios

```yaml
user: "1000:1000"
```

---

# 🖥️ 7. Seguridad del host (MUY IMPORTANTE)

👉 Docker es seguro **solo si el host lo es**

---

## ⚠️ Riesgos críticos

* Acceso al socket Docker
* Kernel vulnerable
* Usuario con permisos excesivos

---

## ✅ Buenas prácticas

### 1. Proteger el socket Docker

❌ Muy peligroso:

```bash
/var/run/docker.sock
```

👉 Da control total del host

---

### 2. No añadir usuarios al grupo docker sin control

```bash
usermod -aG docker usuario
```

👉 Es equivalente a root

---

### 3. Mantener el sistema actualizado

* Kernel
* Docker
* librerías

---

### 4. Usar AppArmor / SELinux

👉 Añaden aislamiento extra

---

### 5. Monitorización

* logs
* métricas
* alertas

Herramientas:

* Prometheus
* Grafana
* Falco

---

### 6. Backup

* Volúmenes
* Configuración

---

# 🧠 8. Errores típicos (muy preguntados)

* ❌ Usar `latest`
* ❌ Ejecutar como root
* ❌ Exponer todos los puertos
* ❌ Meter secretos en imágenes
* ❌ Usar `--privileged`
* ❌ No escanear imágenes
* ❌ Montar todo el sistema (`/`)

---

# 🚀 9. Nivel PRO (muy valorado)

* ✔️ Usar **distroless images**
* ✔️ Firmar imágenes (Docker Content Trust)
* ✔️ Escaneo en CI/CD
* ✔️ Zero Trust networking
* ✔️ Runtime security (Falco)
* ✔️ Kubernetes security policies (si escalas)

---

# 📌 10. Resumen rápido tipo examen

* Imágenes → seguras, ligeras, sin secretos
* Contenedores → no root, sin privilegios
* Red → mínima exposición
* Volúmenes → controlados
* Compose → variables seguras
* Host → actualizado y protegido

---

Si quieres, el siguiente paso puede ser MUY útil:

* 🔥 **Checklist de seguridad real para producción**
* 🧪 **Auditoría de un docker-compose (te digo qué está mal)**
* 💣 **Ejemplos reales de ataques a contenedores (muy interesante)**

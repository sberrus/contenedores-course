
# 🧱 1. Pod — la unidad mínima

El **Pod** es el objeto más pequeño que puedes desplegar en Kubernetes.

👉 No trabajas directamente con contenedores, sino con Pods.

### 🔹 Qué contiene un Pod:

* 1 o varios contenedores
* Red compartida (misma IP y puerto interno)
* Volúmenes compartidos (almacenamiento)

### 🧠 Idea clave:

> Un Pod = “un entorno donde viven contenedores juntos”

### 📌 Ejemplo típico:

* Un contenedor con tu app
* (Opcional) otro contenedor sidecar (logs, proxy, etc.)

⚠️ Problema:
Los Pods **son efímeros** → si mueren, no se recrean solos (si los creas “a mano”).

---

# 🔁 2. Deployment — gestión de Pods

Aquí es donde Kubernetes empieza a ser útil.

Un **Deployment** define:

* Cuántos Pods quieres (réplicas)
* Qué imagen usan
* Cómo se actualizan

### 🔹 Qué hace:

* Mantiene el número deseado de Pods
* Reemplaza Pods que fallan
* Permite **rolling updates** (actualizaciones sin downtime)

### 🧠 Idea clave:

> Deployment = “quiero siempre 3 instancias de esta app funcionando”

Internamente usa un **ReplicaSet** (pero normalmente no lo gestionas tú directamente).

---

# 🌐 3. Service — acceso a los Pods

Los Pods tienen IP dinámica → no puedes depender de ellas.

Un **Service** crea un punto de acceso estable.

### 🔹 Tipos principales:

#### 🔸 ClusterIP (por defecto)

* Solo accesible dentro del cluster
* Comunicación entre microservicios

#### 🔸 NodePort

* Expone el servicio en un puerto del nodo

#### 🔸 LoadBalancer

* Usa un balanceador externo (cloud)

### 🧠 Idea clave:

> Service = “una IP estable que apunta a Pods dinámicos”

---

# 🌍 4. Ingress — entrada HTTP/HTTPS

Para exponer servicios web de forma elegante.

### 🔹 Qué hace:

* Routing HTTP/HTTPS
* Manejo de dominios
* TLS (HTTPS)

### 🧠 Idea clave:

> Ingress = “router web del cluster”

Ejemplo:

* `api.midominio.com` → backend
* `midominio.com` → frontend

---

# ⚙️ 5. ConfigMap y Secret — configuración

Separan configuración del código.

### 🔹 ConfigMap

* Variables no sensibles
* Ej: URLs, flags

### 🔹 Secret

* Datos sensibles
* Ej: passwords, tokens

### 🧠 Idea clave:

> No hardcodees config en la imagen Docker

---

# 💾 6. Volumes — almacenamiento

Los Pods son efímeros → necesitas persistencia.

### 🔹 Tipos:

* emptyDir (temporal)
* hostPath (host)
* PersistentVolume (PV)
* PersistentVolumeClaim (PVC)

### 🧠 Idea clave:

> Volume = “disco accesible desde el Pod”

---

# 📦 7. Namespace — organización

Permite dividir el cluster en entornos lógicos.

### 🔹 Ejemplos:

* dev
* staging
* prod

### 🧠 Idea clave:

> Namespace = “carpeta lógica dentro del cluster”

---

# 🔐 8. ServiceAccount + RBAC — permisos

Controlan qué puede hacer cada componente.

### 🔹 ServiceAccount

* Identidad dentro del cluster

### 🔹 RBAC

* Define permisos (roles)

### 🧠 Idea clave:

> Seguridad interna del cluster

---

# 🔄 Cómo encaja TODO (visión global)

Imagina esto:

1. Creas un **Deployment**
2. Este crea **Pods**
3. Un **Service** expone esos Pods
4. Un **Ingress** permite acceso desde fuera
5. Configuración viene de **ConfigMap/Secret**
6. Datos persistentes → **Volumes**

---

# 🧭 Mapa mental rápido

```
Usuario → Ingress → Service → Pods (gestionados por Deployment)
                               ↓
                        Volumes / Config / Secrets
```

---

# ⚠️ Errores típicos de principiantes

* Crear Pods directamente (en vez de Deployments)
* No usar readiness/liveness probes
* Hardcodear configuración
* No limitar recursos (CPU/memoria)
* Exponer servicios sin control

---

Si quieres, el siguiente paso ideal es:
👉 montar un ejemplo real completo (Deployment + Service + Ingress en YAML)
o
👉 comparar Docker Compose vs Kubernetes (te va a aclarar muchísimo todo)

¿Hacia dónde quieres ir?

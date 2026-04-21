
# 🧠 1. ¿Qué es realmente un Pod en términos de Docker?

Si vienes de Docker, piensa esto:

> 🔑 **Un Pod ≈ un grupo de contenedores Docker que comparten red + almacenamiento y se gestionan como una unidad**

Pero hay una diferencia clave:

👉 En Docker tú gestionas contenedores individuales
👉 En Kubernetes **el Pod es la unidad mínima, no el contenedor**

---

## 🔍 Equivalencia mental rápida

| Kubernetes | Docker equivalente                              |
| ---------- | ----------------------------------------------- |
| Pod        | Grupo de contenedores en la misma red + volumen |
| Container  | Contenedor Docker                               |
| Deployment | Algo tipo docker-compose + auto-healing         |

---

# 🌐 2. Red en un Pod (clave para entenderlo)

En Docker:

* Cada contenedor → IP propia
* Se comunican por red Docker

En Kubernetes (Pod):

* Todos los contenedores del Pod comparten:

  * **misma IP**
  * **mismo namespace de red**

👉 Esto significa:

```bash
localhost:puerto
```

funciona ENTRE contenedores dentro del mismo Pod.

### 🧠 Ejemplo mental:

Imagina dos contenedores dentro de un Pod:

* app → puerto 3000
* sidecar → puerto 8080

👉 El contenedor `app` puede llamar a:

```
http://localhost:8080
```

⚠️ Esto en Docker normal no pasa sin configuración explícita.

---

# 💾 3. Volúmenes compartidos

Dentro de un Pod:

* Los contenedores pueden montar el **mismo volumen**
* Comparten archivos fácilmente

👉 Ejemplo típico:

* Contenedor 1: genera logs
* Contenedor 2: los recoge y los envía (sidecar)

---

# 🧩 4. ¿Por qué varios contenedores en un Pod?

No es lo habitual (normalmente 1 contenedor), pero hay patrones muy importantes:

---

## 🔹 Patrón Sidecar (el más importante)

Un contenedor “acompaña” al principal.

### Ejemplo:

* app (tu API)
* sidecar (envía logs a Elastic Stack)

👉 Ventaja:

* Separas responsabilidades
* No ensucias tu app principal

---

## 🔹 Proxy / networking

* app
* proxy (como Envoy)

👉 Muy usado en service mesh

---

## 🔹 Init containers

Contenedores que se ejecutan ANTES:

* Preparan datos
* Hacen migraciones
* Esperan dependencias

---

# ⚙️ 5. ¿Cómo se configura un Pod?

Aquí es donde Kubernetes cambia el paradigma: todo es declarativo (YAML).

---

## 🧾 Ejemplo simple de Pod con varios contenedores

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: ejemplo-pod
spec:
  containers:
    - name: app
      image: node:18
      ports:
        - containerPort: 3000

    - name: sidecar-logs
      image: busybox
      command: ["sh", "-c", "tail -f /logs/app.log"]
      volumeMounts:
        - name: logs
          mountPath: /logs

  volumes:
    - name: logs
      emptyDir: {}
```

---

## 🔍 Qué está pasando aquí:

* 2 contenedores dentro del mismo Pod
* Comparten volumen (`logs`)
* Pod = unidad de despliegue

---

# 🔄 6. ¿Cómo se gestionan configuraciones?

Aquí entran dos piezas clave:

---

## 🔹 Variables de entorno (como Docker)

Igual que en Docker:

```yaml
env:
  - name: DB_HOST
    value: "mysql"
```

---

## 🔹 ConfigMap (config externa)

```yaml
envFrom:
  - configMapRef:
      name: mi-config
```

👉 Equivalente a `.env` pero gestionado por Kubernetes

---

## 🔹 Secret (datos sensibles)

```yaml
envFrom:
  - secretRef:
      name: mi-secret
```

👉 Para passwords, tokens, etc.

---

## 🔹 Montar config como archivos

```yaml
volumeMounts:
  - name: config
    mountPath: /app/config
```

---

# ⚠️ 7. Cosas IMPORTANTES que cambian respecto a Docker

## ❌ No trates Pods como contenedores

* No los “reinicias”
* Kubernetes los reemplaza

---

## ❌ No son persistentes

* Si muere → se recrea (por Deployment)
* Nueva IP
* Nuevo estado

---

## ❌ No escalan solos

* Escalas Deployments, no Pods

---

# 🧠 8. Resumen mental potente

Si vienes de Docker:

> 🐳 Docker: “ejecuto contenedores”
> ☸️ Kubernetes: “defino cómo quiero que se ejecuten grupos de contenedores (Pods)”

Y más concretamente:

> 🔑 **Pod = entorno compartido (red + storage) donde viven uno o varios contenedores muy acoplados**

---

# 💡 Analogía final

* Contenedor → proceso
* Pod → máquina virtual ligera
* Deployment → autoscaler + gestor

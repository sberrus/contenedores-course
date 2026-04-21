
# 🧠 1. ¿Qué es un Deployment realmente?

Un **Deployment** es un objeto que define **el estado deseado de tu aplicación** y se encarga de que ese estado se cumpla continuamente.

👉 En términos simples:

> “Quiero 3 instancias de mi app corriendo SIEMPRE con esta configuración”

Y Kubernetes se encarga del resto.

---

# 🔁 2. ¿Qué hace internamente?

Un Deployment **no crea Pods directamente**. La cadena real es:

```
Deployment → ReplicaSet → Pods
```

### 🔹 Flujo:

1. Defines un Deployment
2. Kubernetes crea un **ReplicaSet**
3. El ReplicaSet crea y mantiene los Pods

---

## 🧠 ¿Por qué esta capa extra?

Porque permite:

* versionado
* rollback
* actualizaciones progresivas

---

# 🐳 3. Traducción mental a Docker

Esto te va a ayudar mucho:

| Kubernetes | Docker equivalente                                        |
| ---------- | --------------------------------------------------------- |
| Pod        | Contenedor                                                |
| Deployment | docker-compose + auto-healing + scaling + rolling updates |

👉 Docker Compose levanta cosas
👉 Deployment **las mantiene vivas, actualiza y escala automáticamente**

---

# ⚙️ 4. Ejemplo de Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mi-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: mi-app
  template:
    metadata:
      labels:
        app: mi-app
    spec:
      containers:
        - name: app
          image: nginx:latest
          ports:
            - containerPort: 80
```

---

## 🔍 Qué está pasando:

* `replicas: 3` → quieres 3 Pods
* `template` → define cómo son esos Pods
* `selector` → cómo los identifica

---

# ❤️ 5. Lo realmente importante: Auto-healing

Este es uno de los pilares de Kubernetes.

👉 Si un Pod muere:

* El ReplicaSet detecta que faltan Pods
* Crea uno nuevo automáticamente

👉 Si un nodo cae:

* Kubernetes reprograma los Pods en otro nodo

---

## 🧠 Idea clave:

> No gestionas fallos → Kubernetes los corrige

---

# 📈 6. Escalado

Puedes cambiar el número de réplicas:

```bash
kubectl scale deployment mi-app --replicas=5
```

👉 Kubernetes:

* crea nuevos Pods
* los distribuye en nodos

---

## 🔹 Autoescalado (HPA)

Basado en CPU/memoria:

```bash
kubectl autoscale deployment mi-app --min=2 --max=10 --cpu-percent=80
```

---

# 🔄 7. Rolling Updates (clave en producción)

Cuando cambias la imagen:

```yaml
image: nginx:1.25
```

👉 Kubernetes NO mata todo de golpe.

Hace esto:

1. Crea nuevos Pods
2. Va eliminando los antiguos progresivamente
3. Mantiene el servicio disponible

---

## 🧠 Resultado:

> Deploy sin downtime

---

# ⏪ 8. Rollback automático

Si algo falla:

```bash
kubectl rollout undo deployment mi-app
```

👉 Kubernetes vuelve a la versión anterior

---

# 🔍 9. Estrategias de despliegue

Por defecto:

## 🔹 RollingUpdate

Controlas:

```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxUnavailable: 1
    maxSurge: 1
```

* `maxUnavailable`: cuántos Pods pueden caer
* `maxSurge`: cuántos extras se crean

---

## 🔹 Recreate

* Mata todo
* Crea nuevos Pods

👉 Solo útil si no puedes tener dos versiones a la vez

---

# ⚠️ 10. Cosas importantes que mucha gente no entiende

---

## ❌ No se gestionan Pods directamente

👉 Nunca hagas esto en producción:

```bash
kubectl delete pod ...
```

Porque:

* El Deployment lo recreará

---

## ❌ Cambiar Pods “a mano” no sirve

👉 Cambios en Pods se pierden

Siempre modifica el Deployment.

---

## ❌ El estado deseado manda

Si dices:

```yaml
replicas: 3
```

👉 Kubernetes SIEMPRE intentará tener 3, pase lo que pase.

---

# 🔗 11. Cómo encaja con el resto

Flujo completo:

```
Deployment
   ↓
ReplicaSet
   ↓
Pods
   ↓
Service (los expone)
   ↓
Ingress (acceso externo)
```

---

# 🧠 12. Resumen potente

> 🔑 **Deployment = el “manager” que asegura que tu app siempre esté corriendo como tú defines**

Te da:

* Alta disponibilidad
* Auto-reparación
* Escalado
* Deploys sin downtime
* Rollbacks

---

# 💡 Analogía final

* Pod → trabajador
* ReplicaSet → supervisor
* Deployment → jefe que define el equipo y cómo cambia con el tiempo

## 🧠 ¿Qué hace Kubernetes realmente?

* Distribuye contenedores en múltiples máquinas (nodos)
* Escala aplicaciones automáticamente (horizontalmente)
* Se recupera de fallos (reinicia contenedores, reprograma pods)
* Balancea carga entre servicios
* Gestiona configuraciones y secretos
* Permite despliegues sin downtime (rolling updates)

Su unidad básica no es el contenedor directamente, sino el **Pod** (uno o varios contenedores que comparten red y almacenamiento).

---

## ⚙️ Arquitectura (muy resumida)

* **Control Plane (Master)**: toma decisiones (scheduler, API, etc.)
* **Nodos (Workers)**: ejecutan los contenedores
* Componentes clave:

  * kubelet
  * kube-scheduler
  * etcd (base de datos del cluster)

---

## 🧪 Variantes y distribuciones de Kubernetes

Aquí es donde entra lo interesante 👇

### 🔹 1. Kubernetes “puro” (upstream)

La versión oficial mantenida por la CNCF. Es potente pero más compleja de instalar y mantener.

---

### 🔹 2. Kubernetes local / desarrollo

#### 📦 Minikube

* Ejecuta un cluster Kubernetes en local (VM o contenedor)
* Ideal para aprendizaje y pruebas
* Fácil de instalar

#### 📦 Kind

* Ejecuta clusters dentro de contenedores Docker
* Muy usado en testing y CI/CD

#### 📦 k3s

* Kubernetes ligero (menos consumo de recursos)
* Perfecto para IoT, edge computing o labs

---

### 🔹 3. Kubernetes gestionado (cloud)

Servicios donde el proveedor gestiona el control plane:

* Google Kubernetes Engine (GCP)
* Amazon EKS (AWS)
* Azure Kubernetes Service (Microsoft)

👉 Ventaja: no gestionas infraestructura crítica
👉 Desventaja: menor control y dependencia del proveedor

---

### 🔹 4. Alternativas más simples (Kubernetes-like)

Estas no son Kubernetes, pero cubren casos más sencillos:

#### 🐳 Docker Swarm

* Orquestador nativo de Docker
* Mucho más simple que Kubernetes
* Menos potente y menos usado actualmente

#### ⚙️ Nomad

* Más simple que Kubernetes
* Flexible (no solo contenedores)
* Buena integración con herramientas de HashiCorp

---

### 🔹 5. Distribuciones empresariales

* OpenShift (de Red Hat)
* Rancher (gestión multi-cluster)
* VMware Tanzu

👉 Añaden:

* Seguridad reforzada
* UI
* herramientas DevOps integradas

---

## 🧭 ¿Cuál usar según tu caso?

* Aprender → Minikube / Kind / k3s
* Producción pequeña → k3s o managed (EKS, GKE, AKS)
* Producción grande → Kubernetes gestionado o enterprise
* Algo simple → Docker Swarm o Nomad

---

## 💡 Idea clave para quedarte

* **Docker = crear contenedores**
* **Kubernetes = orquestarlos a escala**

---

Si quieres, en el siguiente paso podemos:

* desmenuzar conceptos clave (Pod, Deployment, Service…)
* o hacer un flujo real: *de Docker Compose → Kubernetes* (muy útil para entenderlo de verdad)

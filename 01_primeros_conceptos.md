# Contenedores

## ¿Qué es un contenedor?

Un contenedor es una unidad ligera, portable y ejecutable que incluye:
- Código de la aplicación
- Dependencias
- Librerías
- Configuración

Permite ejecutar aplicaciones de forma consistente en cualquier entorno.

---

## ¿Cómo funcionan?

Los contenedores utilizan características del kernel de Linux:

- **Namespaces** → Aislamiento (procesos, red, usuarios)
- **cgroups (Control Groups)** → Limitación de recursos (CPU, RAM)
- **Union File Systems** → Sistema de capas (imágenes)

A diferencia de máquinas virtuales:
- No necesitan un sistema operativo completo
- Comparten el kernel del host

---

## Arquitectura: Kernel vs Microkernel

### Kernel monolítico (Linux)
- Todo corre en el mismo espacio del kernel
- Más rápido
- Menos aislamiento interno

### Microkernel
- Divide funcionalidades en procesos independientes
- Más modular
- Más seguro, pero con overhead

👉 Docker y contenedores funcionan sobre **kernels monolíticos (Linux)**

---

## Contenedores vs Máquinas Virtuales

| Característica | Contenedores | Máquinas Virtuales |
| -------------- | ------------ | ------------------ |
| Peso           | Ligero       | Pesado             |
| Arranque       | Rápido       | Lento              |
| Aislamiento    | Medio        | Alto               |
| Kernel         | Compartido   | Independiente      |

---

## Ventajas

- Portabilidad
- Escalabilidad
- Consumo reducido
- Ideal para microservicios

---

## Desventajas

- Menor aislamiento que VM
- Dependencia del kernel
- Seguridad más compleja si no se configura bien
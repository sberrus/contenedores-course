# Docker Compose

## Qué es
Define múltiples contenedores en YAML

## Ejemplo
version: "3"
services:
  web:
    build: .
    ports:
      - "8080:80"

## Conceptos
- services
- volumes
- networks

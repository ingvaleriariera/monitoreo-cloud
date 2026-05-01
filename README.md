# 🚀 Sistema de Monitoreo de Infraestructura (Stack LGP modificado)

## 📝 Descripción

Proyecto de monitoreo basado en el stack Prometheus + Grafana + Node Exporter, implementado con Docker y Docker Compose. Este repositorio contiene la configuración necesaria para recopilar métricas del host (a través de Node Exporter), almacenarlas con Prometheus y visualizarlas en Grafana.

## 🏗️ Arquitectura

Flujo de datos:

- Host → Node Exporter: exposa métricas del sistema operativo (CPU, memoria, disco, red).
- Node Exporter → Prometheus: Prometheus scrappea las métricas expuestas por Node Exporter.
- Prometheus → Grafana: Grafana consume los datos de Prometheus para crear dashboards visuales.

Diagrama simple:

Host (node_exporter) → Prometheus (scrape) → Grafana (visualización)

## ✅ Requisitos

- Docker (mínimo recomendado: 20.x)
- Docker Compose (v2+)

Asegúrate de tener ambos instalados y que el usuario pueda ejecutar comandos de Docker.

## 🛠️ Instalación y Despliegue

1. Clonar el repositorio:

```bash
git clone https://github.com/ingvaleriariera/monitoreo-cloud.git
cd monitoreo-cloud
```

2. Iniciar los servicios con Docker Compose:

```bash
docker-compose up -d
```

3. Verificar que los contenedores estén en ejecución:

```bash
docker-compose ps
```

4. Logs de un servicio (ejemplo Prometheus):

```bash
docker-compose logs -f prometheus
```

## ⚙️ Configuración de Grafana

1. Accede a Grafana en tu navegador (por defecto: `http://localhost:3005`).
2. Añade una data source de tipo **Prometheus** con la URL:

```
http://prometheus:9090
```

> Nota: si accedes desde el host y tu `docker-compose` mapea puertos, usa `http://localhost:9090` para probar la conexión desde tu navegador, pero dentro de la red de Docker la URL `http://prometheus:9090` es la correcta.

3. Importar dashboard recomendado:

- Dashboard ID: `1860` (Node Exporter Full)
- En Grafana: *Create* → *Import* → pegar `1860` → seleccionar la data source Prometheus creada.

## 🧪 Prueba de Estrés

Para generar carga y verificar que las métricas suben correctamente puedes usar `stress-ng` dentro de un contenedor:

```bash
docker run --rm -it alexeiled/stress-ng --cpu 4 --timeout 60s
```

Observa en Grafana cómo aumentan las métricas de CPU, load y temperatura (si aplica) durante la prueba.

## 🔎 Comprobaciones rápidas

- Prometheus UI: `http://localhost:9090` (o desde Docker: `http://prometheus:9090`)
- Grafana UI: `http://localhost:3000`
- Node Exporter: normalmente expone métricas en el endpoint `/metrics` (puedes comprobar con `curl` al puerto correspondiente si está mapeado).

## 📦 Archivos principales

- `docker-compose.yml` — Orquestación de contenedores.
- `prometheus.yml` — Configuración de Prometheus (targets, scrape intervals).

## 🎓 Créditos

Proyecto realizado por **Massiel Perozo** y **Valeria Riera** para la asignatura de Computación en la Nube (UCAB).

---


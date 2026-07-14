# Network Monitor (Observability Stack)

> **Note on RPM System**: This monitoring stack is designed to run alongside the core RPM application. You can find the main **Remote Patient Monitoring System (RPM) v2** repository here: [rpm-system-v2](https://github.com/rohith1612/rpm-system-v2). 

## Overview

The `network_monitor` provides a comprehensive observability platform using an open-source stack. It collects, processes, and visualizes metrics, logs, and distributed traces from your services.

## Architecture & Tech Stack

The stack is orchestrated using Docker Compose and consists of the following components:

- **OpenTelemetry Collector (Otel)**: Receives telemetry data (metrics, logs, traces) via OTLP and exports them to the respective backends.
- **Prometheus**: Time-series database for scraping and storing metrics.
- **Loki**: Highly available, multi-tenant log aggregation system.
- **Tempo**: High-volume, distributed tracing backend.
- **Grafana**: The visualization and analytics platform that connects to Prometheus, Loki, and Tempo to provide unified dashboards.

---

## Prerequisites

- **Docker**
- **Docker Compose**

Make sure the Docker daemon is running on your machine before starting the setup.

---

## Setup & Installation

The entire monitoring stack is containerized and can be spun up using a single Docker Compose command.

### 1. Clone the repository

```bash
git clone https://github.com/aaryannampoothiri/network_monitor.git
cd network_monitor
```

### 2. Start the Observability Stack

Navigate to the `observability` directory where the `docker-compose.yml` file is located, and bring up the containers:

```bash
cd observability
docker-compose up -d
```

This will pull the necessary images and start all the services in detached mode.

### 3. Verify Running Services

You can check the status of the containers to ensure everything is running correctly:

```bash
docker-compose ps
```

You should see `otel-collector`, `prometheus`, `loki`, `tempo`, and `grafana` listed as `Up`.

---

## Accessing the Services

Once the stack is running, you can access the services on your local machine using the following ports:

| Service | Port | Local URL | Description |
| :--- | :--- | :--- | :--- |
| **Grafana** | `3000` | [http://localhost:3000](http://localhost:3000) | UI for Dashboards, Logs, and Traces. (Default login is usually `admin` / `admin` unless configured otherwise) |
| **Prometheus** | `9090` | [http://localhost:9090](http://localhost:9090) | Prometheus Web UI for querying metrics |
| **Otel Collector (gRPC)**| `4317` | `localhost:4317` | OTLP gRPC receiver for your applications |
| **Otel Collector (HTTP)**| `4318` | `http://localhost:4318`| OTLP HTTP receiver for your applications |
| **Loki** | `3100` | `http://localhost:3100`| Log aggregation API endpoint |
| **Tempo** | `3200` | `http://localhost:3200`| Tracing API endpoint |

---

## Stopping the Stack

To stop and remove the containers, networks, and volumes (optional):

```bash
cd observability

# Stop the containers without deleting data
docker-compose stop

# OR: Stop and remove containers (data in volumes will persist)
docker-compose down

# OR: Stop, remove containers, AND delete all persisted data volumes
docker-compose down -v
```

## Configuring Dashboards

Pre-provisioned dashboards are loaded into Grafana on startup from the `./observability/grafana/dashboards/` directory. If you want to add new default dashboards, you can place their JSON definitions in that folder and restart Grafana.

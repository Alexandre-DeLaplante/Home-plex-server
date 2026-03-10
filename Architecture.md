# Home Plex Server Architecture

## Overview

This document describes the architecture of the home lab server hosting:

- Network Attached Storage (NAS)
- Plex Media Server
- Monitoring stack with Prometheus, Grafana, and Node Exporter
- Docker containerization

The design emphasizes:

- Centralized storage
- Media streaming across devices
- System monitoring and observability
- Reproducible deployment via Docker Compose

---

## Hardware Architecture

| Component | Description |
|-----------|-------------|
| Server | Linux-based server with adequate CPU and RAM for Plex transcoding |
| Storage | Two SSDs (`ssd1`, `ssd2`) mounted at `/mnt/ssd1` and `/mnt/ssd2` |
| Network | Gigabit LAN for local access |
| UPS | Optional, for power protection |

---

## Software Stack

| Layer | Software | Purpose |
|-------|---------|--------|
| OS | Linux (Ubuntu/Debian) | Host OS |
| Container Engine | Docker & Docker Compose | Container management |
| NAS Service | Samba | File sharing over LAN |
| Media Server | Plex Media Server | Streaming movies and TV shows |
| Monitoring | Prometheus | Collect metrics from Node Exporter |
| Visualization | Grafana | Display dashboards for monitoring |
| Metrics Agent | Node Exporter | Expose hardware metrics (CPU, RAM, Disk, Network) |

---

## Directory Structure

The server directory structure is designed for clarity and reproducibility:

```text
/home-plex-server/
│
├ docker-compose.yml
├ plex/
│   └ config/
├ monitoring/
│   ├ prometheus/prometheus.yml
│   └ grafana/
│       ├ dashboards/node-exporter.json
│       └ provisioning/
│           ├ datasources/datasource.yml
│           └ dashboards/dashboards.yml
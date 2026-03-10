# First Time Setup Guide

## Overview

This guide explains how to deploy and initialize the Plex Media Server and monitoring stack for the first time using Docker.

The system uses:

- Docker for containerization
- Plex Media Server for media streaming
- Prometheus for monitoring metrics
- Grafana for system dashboards
- Node Exporter for hardware monitoring

---

## Prerequisites

Before starting, ensure the server has the following installed:

- Linux (Ubuntu or Debian recommended)
- Docker
- Docker Compose
- Mounted storage drives for media

Verify Docker installation:

docker --version

Verify Docker Compose:

docker-compose version

---

## Clone the Repository

Clone the project to the server:

git clone https://github.com/Alexandre-DeLaplante/Home-plex-server.git

Navigate to the project directory:

cd home-plex-server

---

## Configure Storage Mounts

The Plex server expects media storage to be mounted on the system.

Example mounts:

/mnt/ssd1
/mnt/ssd2

Verify drives are mounted:

ls /mnt

Example output:

ssd1
ssd2

These drives will store the media libraries used by Plex.

---

## Verify Docker Compose Configuration

Open the docker compose file:

nano docker-compose.yml

Ensure the Plex volumes point to the correct storage paths.

Example:

volumes:
  - /mnt/ssd1:/movies
  - /mnt/ssd2:/tv
  - ./plex/config:/config

The config directory will store Plex configuration files.

---

## Start the Server Stack

Start all services:

docker-compose up -d

This command will start:

- Plex Media Server
- Prometheus
- Grafana
- Node Exporter

Verify containers are running:

docker ps

---

## First Plex Login

Open a browser and go to:

http://SERVER-IP:32400/web

Example:

http://192.168.1.50:32400/web

Sign in with your Plex account.

---

## Initial Plex Configuration

During first launch you will:

1. Name the server
2. Sign into your Plex account
3. Create media libraries

Recommended libraries:

Movies  
TV Shows

---

## Add Media Libraries

Add libraries using the following paths:

Movies library:

/movies

TV Shows library:

/tv

Plex will automatically scan and index the media files.

---

## Verify Monitoring System

Access Grafana:

http://SERVER-IP:3000

Default login:

admin
admin

Add Prometheus as the datasource if not already configured.

Import the Node Exporter dashboard (ID: 1860).

---

## Verify System Metrics

Once configured, Grafana should display:

- CPU usage
- RAM usage
- disk usage
- network activity
- system load

Metrics are collected from Node Exporter through Prometheus.

---

## Restarting the Stack

To restart all services:

docker-compose restart

To stop all services:

docker-compose down

To start again:

docker-compose up -d

---

## Updating Containers

To update all services:

docker-compose pull
docker-compose up -d

This will pull newer container versions if available.

---

## Completion

Once the setup is complete, the server will automatically:

- start containers on boot
- monitor system resources
- serve media through Plex
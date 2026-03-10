# Home-plex-server

## NAS + Plex Media Server Homelab

### Overview
This project documents the design, deployment, and management of a home NAS and Plex media server.  
The goal is to create a reliable, centralized storage system with media streaming capabilities and secure online access.

---

## Features
- Centralized NAS storage for movies, TV shows, and backups (multiple separate disks)  
- Plex Media Server running in Docker for reliable deployment  
- Samba file shares for network access  
- Monitoring stack with Grafana dashboards for metrics visualization  

---

## Hardware
- Dedicated Linux server (CPU and RAM sized for Plex performance)  
- Multiple HDDs for media storage (currently separate, not merged)  
- Gigabit network connectivity  
- UPS for power protection  

---

## Software
- Linux server (Ubuntu / Debian)  
- Plex Media Server in Docker  
- Samba for network file sharing  
- Docker & Docker Compose  
- Grafana for monitoring  
- Prometheus and Node Exporter for host metrics  

---

## Network Architecture
- NAS and Plex integrated into the home network
- Docker containers use host networking for Plex for full LAN access and discovery  
- Samba shares provide access to each storage disk  
- Future VLAN/network segmentation planned for security  

---

## Current Implementation

### NAS Storage
- Multiple disks mounted separately (`/mnt/SSD1`, `/mnt/SSD2`)  
- exFAT disks configured with proper UID/GID permissions for Plex and Samba  
- Samba shares created for each disk to allow read/write access  

### Plex Media Server (Docker)
- Plex container runs in **host network mode**  
- Config directory stored persistently on host (`/home/user/home-plex-server/plex/config`)  
- Media volumes mounted individually:  
  - `/mnt/ssd1:/media1`  
  - `/mnt/ssd2:/media2`  
- Docker Compose ensures automatic restarts  

### Monitoring Stack
- Grafana dashboards display CPU, RAM, disk usage, and Docker container metrics  
- Prometheus and Node Exporter optional for detailed host-level metrics  

### Permissions
- exFAT disks mounted with `uid=1000,gid=1000,fmask=002,dmask=002`  
- Samba configured with `force user` and `force group` to match Plex UID/GID  
- Ensures seamless read/write access from Plex and network clients  

---

## Future Implementations

- **Network Segmentation & Security**: Place NAS and Plex on separate VLANs or subnets, apply firewall rules, and restrict access to only necessary services.  
- **Reverse Proxy & Secure External Access**: Use NGINX as a reverse proxy with HTTPS (Let’s Encrypt) and authentication for safe remote access.  
- **Storage Reliability & Redundancy**: Implement RAID RAID1 for data redundancy and protection against disk failure.  
- **Automated Backups**: Schedule regular backups of media and Plex configuration to local or cloud storage.   
- **Hardware Acceleration & Performance Optimization**: Enable GPU/CPU transcoding in Plex to handle multiple streams efficiently, and optimize Docker container resource limits.  
- **Scalability & Expandability**: Design storage and Docker services to allow easy addition of new disks or services (e.g., Sonarr, Radarr, Jellyfin) without downtime.  
- **Authentication & Access Control**: Enforce proper user permissions for Samba shares and Plex, using dedicated service accounts where possible.  


## Setup Instructions

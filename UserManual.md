# User Manual

## Overview
This server provides two primary services:

- Network Attached Storage (NAS) for centralized file storage
- Plex Media Server for streaming media across devices

The system is designed to run continuously and automatically restart services when the server boots.

---

## Accessing the NAS

The NAS is accessible from devices on the local network using SMB (Samba).

### Windows
1. Open File Explorer
2. Click **Map Network Drive**
3. Enter the server address:

\\SERVER-IP\nas

4. Authenticate with the server credentials if prompted.

Example:

\\192.168.1.50\nas

---

## Accessing Plex

Open a browser and navigate to:

http://SERVER-IP:32400/web

Example:

http://192.168.1.50:32400/web

Sign in with your Plex account to access the media libraries.

---

## Media Library Structure

Media files should be organized in the following format for proper Plex indexing.

### Movies

/movies/Movie Name (Year)/Movie Name (Year).mp4

Example:

/movies/Interstellar (2014)/Interstellar (2014).mp4

### TV Shows

/tv/Show Name/Season 01/Show Name - S01E01.mkv

Example:

/tv/Breaking Bad/Season 01/Breaking Bad - S01E01.mkv

---

## Adding New Media

1. Connect to the NAS share.
2. Navigate to the appropriate folder:
   - Movies
   - TV Shows
3. Copy the media files into the correct directory.
4. Plex will automatically detect new content.

If needed, open Plex and click **Scan Library Files**.

---

## Monitoring the Server

The system monitoring dashboard can be accessed at:

http://SERVER-IP:3000

Example:

http://192.168.1.50:3000

Login credentials are configured during Grafana setup.

The monitoring system tracks:

- CPU usage
- memory usage
- disk utilization
- network activity
- system uptime

---

## Restarting Services

If services need to be restarted, connect to the server via SSH and run:

docker compose restart

This will restart all containers including:

- Plex
- Prometheus
- Grafana
- Node Exporter

---

## Stopping the Server

To safely shut down the server:

sudo shutdown now

This prevents filesystem corruption.

---

## Backup Recommendations

Important data should be backed up regularly, including:

- Plex configuration
- media library
- server configuration files

Backup strategies may include:

- external drives
- offsite backups
- RAID storage

---

## Troubleshooting

### Plex Not Loading
Check if the container is running:

docker ps -a

Restart Plex if necessary:

docker restart plex

---

### NAS Share Not Accessible
Restart the Samba service:

sudo systemctl restart smbd

---

### Monitoring Dashboard Not Loading
Restart the monitoring stack:

docker restart grafana prometheus

---

## Future Improvements

Planned improvements to the system include:

- network segmentation
- reverse proxy with HTTPS
- firewall hardening
- RAID storage configuration
- remote access security
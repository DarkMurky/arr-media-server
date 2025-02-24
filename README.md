# Arr Media Server
## Open source media managment server

This repository contains a Docker Compose configuration for setting up a media server using various services including qBittorrent-nox, Prowlarr, Radarr, Sonarr, Jellyfin, and Jellyseerr.

- Download your media locally
- Watch your media locally
- 🌬️ Be free 🌬️

Clone the project

```bash
git clone https://github.com/DarkMurky/arr-media-server.git
```

Go to the project directory

```bash
cd  arr-media-server/docker-files
```
Compose using docker:

```bash
docker-compose up -d
```

Enter web setup for each service:

```bash
Jellyfin: http://127.0.0.1:8096
Prowlarr: http:///127.0.0.1:9696
Jellyseer: http:///127.0.0.1:5055
Sonarr: http:///127.0.0.1:8989
Radarr: http:///127.0.0.1:7878
qBittorrent: http:///127.0.0.1:6881
```

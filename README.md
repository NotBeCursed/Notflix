# Notflix
Notflix is a Docker-based stack that provides a complete solution for downloading and streaming movies and TV shows. The stack integrates several popular services: Plex for media management, Sonarr and Radarr for automatic downloads, Jackett for indexers, Overseerr for user requests, and qBittorrent for torrenting.

## Prerequisites

 - A *Plex* account for media streaming.
 - A server running *Docker*.
 - Optional: A *Nginx Proxy Manager* (NPM) for easier reverse proxy management and SSL configuration.

## Installation

1. Clone the Git repository:
```bash
git clone https://github.com/NotBeCursed/Notflix.git
cd Notflix
```

2. Configure the .env file:
Set up your `NOTFLIX_PATH` variable to specify the location where you want to store configuration files and media.
Optionally, you can set the `PLEX_CLAIM` token (if you have it) for easier Plex authentication.

3. Run the Docker containers:
Once you've configured your `.env` file, run the following command to launch the Docker containers in the background:

```bash
docker-compose up -d
This will download the necessary Docker images and start all services.
```

4. Access and configure the services:
 - Plex: Access Plex through `http://localhost:32400/web` and sign in with your account. Set up your libraries and media folders as specified in your docker-compose.yml.
 - Sonarr & Radarr: You can access Sonarr through `http://localhost:8989` and Radarr through `http://localhost:7878`. Configure them to automatically download your desired TV shows and movies.
 - Jackett: Access Jackett via `http://localhost:9117` and configure it with your torrent indexers.
 - Overseerr: Use `http://localhost:5055` to manage requests for new media to be downloaded.
 - qBittorrent: Access the Web UI for qBittorrent at `http://localhost:8080` to monitor and manage your downloads.
 - Flaresolverr: It runs on `http://localhost:8191` and is used to solve captchas for Jackett.

5. Optional Configuration (Nginx Proxy Manager):
If you are using *Nginx Proxy Manager* (NPM), ensure that the necessary ports for each service are open and properly configured. You'll also need to configure reverse proxies in NPM to manage access to your services with SSL.

## Configuration
### Environment Variables:
In the `docker-compose.yml` file, update the following environment variables as needed:

 - *PUID* and *PGID*: Set these to your user and group ID. Commonly, `1000` works for the default user.
 - *TZ*: Set your timezone (e.g., `Europe/Paris`).
 - *PLEX_CLAIM* (optional): If you have a Plex claim token, add it to allow automatic Plex authentication.

### Volumes:
Make sure to map your media directories to the appropriate locations in each service. Here's how you can configure it for Plex:

```yaml
volumes:
  - $NOTFLIX_PATH/config/plex:/config
  - $NOTFLIX_PATH/media/series:/tv
  - $NOTFLIX_PATH/media/movies:/movies
```

### Ports:
You can manage which ports are exposed to the host by modifying the `ports` section for each service. For example, Plex uses port `32400` by default, while Sonarr uses port `8989`.

## Services Overview

 - *Plex*: A media server for streaming your content.
 - *Sonarr*: Automatically downloads TV shows.
 - *Radarr*: Automatically downloads movies.
 - *Jackett*: Connects with torrent indexers for search.
 - *qBittorrent*: Torrent client for downloading media.
 - *Overseerr*: User interface for managing media requests.
 - *Flaresolverr*: CAPTCHA solver to bypass restrictions on some indexers.

## Troubleshooting

 - *Ports not opening*: If you're using Nginx Proxy Manager, ensure the correct ports are exposed in the docker-compose.yml and that NPM is correctly configured to proxy traffic.
 - *Permission issues*: Ensure that the PUID and PGID are correctly set to match your system's user and group IDs.
 - *Container issues*: If a container fails to start, check the logs with:
```bash
docker-compose logs <container_name>
```

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.


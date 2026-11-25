📜 Development Log / Progress

This is a description of the attempt to install openobserve software onto the Lenovo Think centre

## Preparation:
- Lenovo Think centre (Desktop)
- Server OS pre-installed (Ubuntu)

## Decision-Making 
This project is optimized for lightweight edge deployment and minimal resource consumption.

  * **Minimal Storage Footprint:** Reduces the initial disk image size to under 10 GB, leaving maximum storage free for application data and logs.
  * **Low RAM Usage:** Keeps idle memory consumption under 100MB, freeing up system RAM for background services and processing.

## Process

Information about the install was found here [setup](https://docs.victoriametrics.com/)  

### Prerequisites

Before you begin, ensure you have the following installed on your host machine:
* [Docker](https://docs.docker.com/get-docker/)
* [Docker Compose](https://docs.docker.com/compose/install/) *(Note: Modern Docker Desktop and Docker Engine installations include Docker Compose natively via the `docker compose` CLI plugin).*

---

### Why Docker Compose?
Managing VictoriaLogs via Docker Compose provides a clean, self-contained operations model:
* **Zero-Dependency Setup:** No need to manually download binaries, configure `systemd` services, or worry about host OS compatibility. Everything runs in an isolated container.
* **Painless Upgrades:** Updating VictoriaLogs to the latest version requires just two commands, with zero manual file replacement.
* **Safe Data Isolation:** Log storage is fully decoupled from the container lifecycle using Docker volumes, ensuring your log data persists safely across updates and restarts.
* **Declarative Configuration:** All settings, ports, and data paths are version-controlled right inside the `docker-compose.yml` file.

Remoted into the Desktop (SSH)- 
`ssh user@ip` 

I started by making a directory to keep the docker-compose file containedby `mkdir openobserver` && changed the to the correct directory `cd openobserver`

I created a new the docker-compose.yml by using the command `touch docker-compose.yml` &&  `nano docker-compose.yml`.

The configuration is below:
`services:
  openobserve:
    image: o2cr.ai/openobserve/openobserve:latest
    container_name: openobserve
    restart: unless-stopped
    ports:
      - "5080:5080"
    environment:
      ZO_DATA_DIR: /data
      ZO_ROOT_USER_EMAIL: "XXXXX@example.com"
      ZO_ROOT_USER_PASSWORD: "XXXXXXXXXXXXXXX"
    volumes:
      - ./data:/data`

I started the docker compose container up with `docker compose up -d`

When the container i went to to view the page which is located in the url- `http://localhost:5080`

The results can be viewed in the openobservedashboard.png

I will continue to test what this dashboard can track.

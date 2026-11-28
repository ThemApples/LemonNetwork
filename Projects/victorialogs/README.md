📜 Development Log / Progress

This is a description of the attempt to install victorialog software onto the Lenovo Think centre

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

I started by making a directory to keep the docker-compose file containedby `mkdir newvictorialogs` && changed the to the correct directory `cd newvictorialogs`

I created a new the docker-compose.yml by using the command `touch docker-compose.yml` &&  `nano docker-compose.yml`.

The configuration is below:
`services:
  victorialogs:
    image: docker.io/victoriametrics/victoria-logs:latest
    container_name: victorialogs
    restart: unless-stopped
    ports:
      - "9428:9428"
    volumes:
      - ./victoria-logs-data:/victoria-logs-data
    command:
      - "-storageDataPath=/victoria-logs-data"
      - "-retentionPeriod=14d"
      - "-httpListenAddr=:9428"
    healthcheck:
      test: ["CMD", "/victoria-logs-prod", "-healthcheck"]
      interval: 10s
      timeout: 5s
      retries: 3`

I started the docker compose container up with `docker compose up -d`

When the container i went to to view the page which is located in the url- `http://localhost:9428/select/vmui/`

The results can be viewed in the victorialogsdashboard.png

I wanted to test a ping to see if i could see the log. 
`curl -X POST 'http://localhost:9428/insert/jsonline?_stream_fields=app,env&_msg_field=message&_time_field=timestamp' \
  -H 'Content-Type: application/stream+json' \
  -d '{"timestamp":"2026-11-16T04:55:00Z","app":"my-service","env":"prod","level":"info","message":"User logged in successfully"}' `

The results can be viewed in the victorialogAfterping.png.

I have configured log ingestion and confirmed that reports are successfully sending to VictoriaLogs. Next, I'll evaluate whether I can implement lightweight, efficient client-side tracking to capture user browser behavior.


📜 Development Log / Progress

This is a description of the attempt to install wazuh software into the raspberry pi with docker

## Preparation:
- SD card (Preinstalled with Raspberry Pi OS Lite) (64bit)
- Raspberry Pi (Im using the Raspberry Pi 4)

## Decision-Making 
This project is optimized for lightweight edge deployment and minimal resource consumption.

  * **Minimal Storage Footprint:** Reduces the initial disk image size to under 1 GB, leaving maximum storage free for application data and logs.
  * **Low RAM Usage:** Keeps idle memory consumption under 100MB, freeing up system RAM for background services and processing.

## Process

Remoted into the Pi (SSH)- 
`ssh user@ip` 

Updated the Pi - `sudo apt update && upgrade`

Installed docker - `curl -fsSL https://get.docker.com/sh `

Enable the services for docker - `sudo systemctl enable_now docker`

Modified the permissions to the raspberry pi user - `sudo usermod -aG docker $USER`

Restarted the Pi - `sudo reboot`

remoted back into the Pi with the above SSH command.

The concern before setting up is the virutal memory, this would need to be expanded - `sudo sysctl -w vm.max_map_count=262144`

Making the changes permanent - `echo "vm.max_map_count=262144" | sudo tee -a /etc/sysctl.conf`

Started to clone the repo - `git clone https://github.com/wazuh/wazuh-docker.git -b v4.9.2
cd wazuh-docker/single-node`

Generating the required SSL certificates:
 `docker compose -f generate-indexer-certs.yml run --rm generator`

 Waited about 5 minutes for everything to boot up. 

 Bring the docker up - `docker compose up -d`

 Waited another few minutes and the docker files and the server were up. 

 But i couldnt reach open the manager. I tried to troubleshoot using the - `docker ps` command and tried to turn off certain services however the docker containers kept restarting every 20 seconds.

 I bought the services down with - `docker compose down`. Then restarted the Pi. 

 However still Bringing it back up the services is still restarting every 20 seconds.

 I Left it there for the day.

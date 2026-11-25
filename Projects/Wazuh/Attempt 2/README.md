📜 Development Log / Progress

This is a description of the attempt to install wazuh software into the raspberry pi

## Preparation:
- SD card (Preinstalled with Raspberry Pi OS Lite) (64bit)
- Raspberry Pi (Im using the Raspberry Pi 4)

## Decision-Making 
This project is optimized for lightweight edge deployment and minimal resource consumption.

  * **Minimal Storage Footprint:** Reduces the initial disk image size to under 1 GB, leaving maximum storage free for application data and logs.
  * **Low RAM Usage:** Keeps idle memory consumption under 100MB, freeing up system RAM for background services and processing.

## Process

This time I followed this [setup](https://documentation.wazuh.com/current/quickstart.html) guide 

Remoted into the Pi (SSH)- 
`ssh user@ip` 

Removed the previous containers with `sudo rm -rf /var/lib/docker &&
sudo rm -rf /var/lib/containerd`

As recommended to install wazuh with this `curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh && sudo bash ./wazuh-install.sh -a`

The install took about 10 minutes to install. 
However, it started to give an error as stated below

`INFO: Wazuh dashboard web application not yet initialized. Waiting...`

I started to look into the limitation of the raspberry pi and then decided maybe i should install the wazuh agent on this instead of the manager. 

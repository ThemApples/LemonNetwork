📜 Development Log / Progress

This is a description of the attempt to install wazuh software onto the Lenovo Think centre

## Preparation:
- Lenovo Think centre (Desktop)
- Server OS pre-installed (Ubuntu)

## Decision-Making 
This project is optimized for lightweight edge deployment and minimal resource consumption.

  * **Minimal Storage Footprint:** Reduces the initial disk image size to under 10 GB, leaving maximum storage free for application data and logs.
  * **Low RAM Usage:** Keeps idle memory consumption under 100MB, freeing up system RAM for background services and processing.

## Process

This time I followed this [setup](https://documentation.wazuh.com/current/quickstart.html) guide 

Remoted into the Desktop (SSH)- 
`ssh user@ip` 

Removed the previous containers with `sudo apt update && sudo apt upgrade -y`

As recommended to install wazuh with this `curl -sO curl -sO https://packages.wazuh.com/4.x/wazuh-install.sh && sudo bash ./wazuh-install.sh -a`

however i was getting error saying 'access denied'. 

I used vim to check the install file. However it could not retrieve the file again using believing a didnt have proper write permissions.

I viewed the file using nano - `nano wazuh-install.sh`
the file was removed with the remove command- `rm wazuh-install.sh`

However i was still getting the same error meassage. Installed this again except i noticed the version in the url of the package involved a '4.x'

so i changed it to - 'curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh'

Then the script started to run

The install took about 10 minutes to install. 
However, i could not reach the wazuh dashboard, even using the port number 443. 


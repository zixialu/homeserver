# homeserver

## Plans

### Hardware

- Pi 4 (eventually just for pihole)
- Some server (pi for now)
- NAS

### Software

- PiHole
- Fenrus/Heimdall/Homer
- Yacht/Portainer?
- Jellyfin
    - Radarr
    - Sonarr
    - Jackett
- Home Assistant
- Uptime Kuma
- Some torrent client
- Nextcloud

## Resources

https://github.com/awesome-selfhosted/awesome-selfhosted

[What's On My Home Server? Storage, OS, Media, Provisioning, Automation](https://www.youtube.com/watch?v=f5jNJDaztqk)

[What's on my Home Server? MUST HAVE Services!](https://www.youtube.com/watch?v=c4rKWrH88F0)

## Setup

### Raspberry Pi

1. Install Ubuntu Desktop via Raspberry Pi Imager

2. For Raspberry Pi, add the following lines to `/boot/firmware/`:
    ```bash
    # Flip the display
    lcd_rotate=2
    ```
    
3. [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
    
4. [Install Portainer with Docker on Linux](https://docs.portainer.io/start/install/server/docker/linux)

### Pihole

Run Pihole by copying `pihole/docker-compose.yml` to a Portainer custom app template. 
[GitHub - pi-hole/docker-pi-hole: Pi-hole in a docker container](https://github.com/pi-hole/docker-pi-hole#quick-start)

Note that there are some changes that need to be made for Pihole to run on Ubuntu: [https://github.com/pi-hole/docker-pi-hole#installing-on-ubuntu-or-fedora](https://github.com/pi-hole/docker-pi-hole#installing-on-ubuntu-or-fedora)

`TODO`: Revamp this section for running pihole locally when there is a separate server

`TODO`: Instructions for copying config (e.g. local DNS entries)

`TODO`: Instructions for configuring clients

### Home Assistant

Run Home Assistant by copying the `homeassistant/docker-compose.yaml` to another Portainer custom template.
[See docs for details](https://www.home-assistant.io/installation/linux#docker-compose)

`TODO`: Configure Mosquitto and Zigbee2MQTT, MQTT integration in HA

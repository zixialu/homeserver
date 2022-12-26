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

1. Install Ubuntu Desktop via Raspberry Pi Imager
2. For Raspberry Pi, add the following lines to `/boot/firmware/`:
    ```bash
    # Flip the display
    lcd_rotate=2
    ```
    
3. Install docker 
    [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
    
4. Install Portainer with Docker
    [Install Portainer with Docker on Linux](https://docs.portainer.io/start/install/server/docker/linux)
    
5. Run Pihole by copying the `docker-compose.yml` from the `docker-pi-hole` project to a Portainer custom app template. 
    [GitHub - pi-hole/docker-pi-hole: Pi-hole in a docker container](https://github.com/pi-hole/docker-pi-hole#quick-start)
    ```yml
    version: "3"

    # More info at https://github.com/pi-hole/docker-pi-hole/ and https://docs.pi-hole.net/
    services:
      pihole:
        container_name: pihole
        image: pihole/pihole:latest
        # For DHCP it is recommended to remove these ports and instead add: network_mode: "host"
        ports:
          - "53:53/tcp"
          - "53:53/udp"
          - "67:67/udp" # Only required if you are using Pi-hole as your DHCP server
          - "80:80/tcp"
        environment:
          TZ: 'America/Chicago'
          # WEBPASSWORD: 'set a secure password here or it will be random'
        # Volumes store your data between container upgrades
        volumes:
          - './etc-pihole:/etc/pihole'
          - './etc-dnsmasq.d:/etc/dnsmasq.d'
        #   https://github.com/pi-hole/docker-pi-hole#note-on-capabilities
        cap_add:
          - NET_ADMIN # Required if you are using Pi-hole as your DHCP server, else not needed
        restart: unless-stopped
    ```
    
    Note that there are some changes that need to be made for Pihole to run on Ubuntu: [https://github.com/pi-hole/docker-pi-hole#installing-on-ubuntu-or-fedora](https://github.com/pi-hole/docker-pi-hole#installing-on-ubuntu-or-fedora)
    
    `TODO`: Revamp this section for running pihole locally when there is a separate server
    
    `TODO`: Instructions for copying config (e.g. local DNS entries)
    
    `TODO`: Instructions for configuring clients

6. Run Home Assistant by copying the docker compose yml from https://www.home-assistant.io/installation/linux#docker-compose to another Portainer custom template
   ```yml
   version: '3'
   services:
      homeassistant:
        container_name: homeassistant
        image: "ghcr.io/home-assistant/home-assistant:stable"
        volumes:
          - /PATH_TO_YOUR_CONFIG:/config
          - /etc/localtime:/etc/localtime:ro
        restart: unless-stopped
        privileged: true
        network_mode: host
   ```

7. 

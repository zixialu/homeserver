# homeserver

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

Run Pihole by copying `pihole/docker-compose.yml` to a Portainer custom app template. You'll need to change the `WEBPASSWORD` variable to a password of your choosing.
[See docs for details.](https://github.com/pi-hole/docker-pi-hole#quick-start)

Note that there are some changes required for Pihole to run on Ubuntu without running into conflicts on port 53: [https://github.com/pi-hole/docker-pi-hole#installing-on-ubuntu-or-fedora](https://github.com/pi-hole/docker-pi-hole#installing-on-ubuntu-or-fedora)

`TODO`: Revamp this section for running pihole locally when there is a separate server

`TODO`: Config (e.g. local DNS entries)

`TODO`: Configuring clients

### Home Assistant

Run Home Assistant by copying `homeassistant/docker-compose.yaml` to another Portainer custom template.
[See docs for details.](https://www.home-assistant.io/installation/linux#docker-compose)

For Zigbee devices, we will use [Mosquitto](https://mosquitto.org/) and [Zigbee2MQTT](https://www.zigbee2mqtt.io/guide/installation/). To set these up:

1. [Determine the location of your adapter](https://www.zigbee2mqtt.io/guide/installation/01_linux.html#determine-location-of-the-adapter-and-checking-user-permissions). Most of the time this will be `/dev/ttyACM0`. Remember this for later.

2. Copy the provided `homeassistant/mosquotto/mosquitto.conf` to `/opt/mosquitto/config/mosquitto.conf`

3. Start the Mosquitto container in Portainer and open its console through `/bin/sh`. Create a new MQTT user with `mosquitto_passwd -c /mosquitto/config/password.txt hass`. Once this user is created, open `mosquitto.conf` and uncomment the authentication lines (ensure that the `password.txt` file has been created in the same folder), and restart the Mosquitto container.

4. To connect Home Assistant to Mosquitto, open the Home Assistant web interface and navigate to `Settings > Devices & Services`. From here, click `Add integration` in the bottom-right, and search for MQTT. Configure the integration with Mosquitto's IP and port (`192.168.0.101` and `1883` in my case), and the auth credentials you set in the last step.

5. Finally, you'll need to configure Zigbee2MQTT to interface between your physical coordinator and the Mosquitto broker. Copy `homeassistant/zigbee2mqtt/configuration.yaml` to `/opt/zigbee2mqtt/data/configuration.yaml`. Change the MQTT server url to Mosquitto's location, and update the serial port with the location of the adapter you determined earlier. Then, copy `homeassistant/zigbee2mqtt/secret.yaml.example` to `/opt/zigbee2mqtt/data/secret.yaml` and set a username and password inside. Restart the Zigbee2MQTT container and visit port `8099` to begin pairing Zigbee devices.

# Smart Cart V3
Version 3 of the Smart Cart
This repo contains the individual software components that make up Smart Cart V3

## How to setup

### On Device

This assumes you have access to a build smart cart system and are attempting to install on a raspberry pi 5 provided already.

#### Device Backend

1. First build the backend in the sc3-camera-connector repo, using the instructions in the readme for the target architecture, docker files are provided for build environments.
2. Then transfer the build .deb package to the system using scp and install with `sudo apt install ./sc3-camera-connector.deb`.
3. Ensure in this stage Xvfb is setup as a persistent service running before the camera connector is used for the depth features to work. `sudo apt install xvfb`. `Xvfb :99 -screen 0 1920x1080x24 &`, or for more persistence, setup a systemd service and enable it for this to start at boot time.

#### Device Frontend

1. Download the UI package and install in a known folder.
2. Edit the provided config.json to match type unix and location whatever socket is configured (default: `/run/sc3/ui.socket`).
3. Ensure the ui has permissions, and is in the sc3 group, i.e. `sudo usermod -aG sc3 $USER`.
4. Create a systemd service and enable if you want it to start on system boot.

### Off-Device

Dashboard stuff here.


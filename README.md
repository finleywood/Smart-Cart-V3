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

First, download [Node.js v24.11.1 (LTS)](https://nodejs.org/en/download) and verify it is installed:

```bash
node -v
npm -v
```

Next, install and configure Poetry:

```bash
pip install poetry
poetry --version
poetry config virtualenvs.in-project true
```

Next, clone the repository:

```bash
git clone https://github.com/davidafshepherd/smartcart-v3
cd smartcart-v3
```

Next, download the SAM3 model weights from [Hugging Face](https://huggingface.co/facebook/sam3).  
Store the weights within the directory `backend/models/sam3`.

Next, set up the backend:

```bash
cd backend
poetry install
```

Run the backend to verify it is set up:

```bash
poetry run uvicorn app.main:app --reload --port 8000
```

The backend will run at [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs). Press CTRL + C to exit.

Lastly, set up the frontend:

```bash
cd ../frontend
npm install
```
Run the frontend to verify it is set up:

```bash
npm run dev
```

The frontend will run at [http://localhost:3000](http://localhost:3000). Press CTRL + C to exit.


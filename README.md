text
# Docker Setup & Control Guide
*For Raspberry Pi & Linux*

---

## What is Docker?

Docker runs applications in **isolated containers**. Each container acts like a mini computer with its own settings. You can run multiple containers at once, each mapped to different ports.

---

## 1. Installing Docker (Raspberry Pi OS)

Open your terminal and run:
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

text

Add your user to the Docker group (so you don’t have to use `sudo` every time):
sudo usermod -aG docker $USER

Log out or reboot to apply group changes
text

---

## 2. Basic Docker Commands

| Command                                | Description                     |
|-----------------------------------------|---------------------------------|
| `docker ps`                            | List running containers         |
| `docker ps -a`                         | List all containers (incl. stopped) |
| `docker logs <container-name>`          | View container logs             |
| `docker stop <container-name>`          | Stop a container                |
| `docker start <container-name>`         | Start a container               |
| `docker rm <container-name>`            | Remove a container              |
| `docker rmi <image-name>`               | Remove an image                 |

---

## 3. Installing & Managing Containers

### 3.1. Install CasaOS (Example)

Visit [CasaOS Official Site](https://casaos.io) for the latest script.

**Install with:**
curl -fsSL https://get.casaos.io | sudo bash

text

**Access CasaOS in your browser:**
http://<RaspberryPi-IP>:<CasaOS-Port>

text

---

### 3.2. Install Rhasspy

**Create profiles directory:**
mkdir -p $HOME/.config/rhasspy/profiles

text

**Run Rhasspy container:**
docker run -d
--name rhasspy
--restart unless-stopped
-p 12101:12101
-v "$HOME/.config/rhasspy/profiles:/profiles"
--device /dev/snd
rhasspy/rhasspy:latest
--profile en

text

**Access Rhasspy UI:**
http://<RaspberryPi-IP>:12101

text

---

## 4. Adding New Containers

- Always assign a unique port: `-p <host-port>:<container-port>`
- Use descriptive names with `--name`

**Example:**
docker run -d
--name new_app
--restart unless-stopped
-p 12345:80
new_app_image:latest

text

**Access in browser:**
http://<RaspberryPi-IP>:12345

text

---

## 5. Useful Tips

- See running containers and their ports:
docker ps

text
- Containers with `--restart unless-stopped` auto-restart after reboot.

---

## 6. Updating Containers

1. **Stop and remove old container:**
docker stop <container-name>
docker rm <container-name>

text
2. **Pull the latest image:**
docker pull <image-name>:latest

text
3. **Re-run the `docker run` command as above.**

---

## 7. Removing Containers and Images

- **Remove a running container (stop first):**
docker stop <container-name>
docker rm <container-name>

text
- **Remove an image:**
docker rmi <image-name>

text

---

## 8. Troubleshooting

- If a container keeps restarting:
docker logs <container-name>

text
- If you can’t access a container’s web UI:
- Check with `docker ps`
- Use the correct IP and mapped port in your browser

---

## 9. Extending Your Documentation

For every new container you add:
- Save the full `docker run ...` command
- Note container name, mapped port, and internal port
- Write its web access URL for future reference

---

> **Tip:** Update this file as you add more containers & services to keep your Docker stack organized!

---

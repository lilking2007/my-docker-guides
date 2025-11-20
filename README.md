========================================
       Docker Setup & Control Guide
        (For Raspberry Pi & Linux)
========================================

1. What is Docker?
------------------
Docker runs applications in isolated containers. Each container is like a mini computer with its own settings. You can run multiple containers at once, each mapped to different ports.

2. Installing Docker (Raspberry Pi OS)
--------------------------------------
Open your terminal and run:
    curl -fsSL https://get.docker.com -o get-docker.sh
    sudo sh get-docker.sh

Add your user to the Docker group to avoid typing sudo:
    sudo usermod -aG docker $USER
    # Log out or reboot to apply group changes

3. Basic Docker Commands
------------------------
List running containers:
    docker ps

List all containers (including stopped):
    docker ps -a

View container logs:
    docker logs <container-name>

Stop a container:
    docker stop <container-name>

Start a container:
    docker start <container-name>

Remove a container:
    docker rm <container-name>

Remove an image:
    docker rmi <image-name>

4. Installing & Managing Containers
-----------------------------------

4.1. Install CasaOS (Example)
    Visit casaos.io for the latest script.
    Common command:
        curl -fsSL https://get.casaos.io | sudo bash
    Access CasaOS in your browser:
        http://<RaspberryPi-IP>:<CasaOS-Port>

4.2. Install Rhasspy
    Create profiles directory:
        mkdir -p $HOME/.config/rhasspy/profiles

    Run Rhasspy container:
        docker run -d \
            --name rhasspy \
            --restart unless-stopped \
            -p 12101:12101 \
            -v "$HOME/.config/rhasspy/profiles:/profiles" \
            --device /dev/snd \
            rhasspy/rhasspy:latest \
            --profile en

    Access Rhasspy in your browser:
        http://<RaspberryPi-IP>:12101

5. Adding More Containers
-------------------------
Use the same pattern for new containers.
- Assign a unique port: -p <host-port>:<container-port>
- Use descriptive names with --name

Example:
    docker run -d \
        --name new_app \
        --restart unless-stopped \
        -p 12345:80 \
        new_app_image:latest

Access:
    http://<RaspberryPi-IP>:12345

6. Useful Tips
--------------
- See running containers and ports:
      docker ps

- Containers will restart automatically on reboot if you use --restart unless-stopped.

7. Updating Containers
----------------------
1. Stop and remove old container:
      docker stop <container-name>
      docker rm <container-name>

2. Pull the latest image:
      docker pull <image-name>:latest

3. Re-run the `docker run` command as above.

8. Removing Containers and Images
---------------------------------
To remove a running container (stop first):
    docker stop <container-name>
    docker rm <container-name>

To remove an image:
    docker rmi <image-name>

9. Troubleshooting
------------------
- If a container keeps restarting, check logs:
      docker logs <container-name>

- If you can't access a web UI, check with:
      docker ps
  Then use the correct IP:port in your browser.

10. Extending Your Documentation
-------------------------------
- For every new container:
    - Note the full docker run command
    - Record container name, internal port, mapped host port
    - Write its browser access URL

========================================
Update this file as you add more containers/services.
========================================

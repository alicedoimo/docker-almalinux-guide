# Installing Docker and Running AlmaLinux 9 on Ubuntu 22.04 LTS

Docker is a robust containerization platform that lets you package applications and their dependencies into portable, lightweight containers. This guide walks you through installing Docker on Ubuntu 22.04 LTS and running AlmaLinux 9 within a container.

> **Note:** Although these instructions are written for Ubuntu 22.04 LTS, they work similarly on later versions (e.g., Ubuntu 24.04 LTS).

## System Requirements

Before installing Docker, confirm that your system meets these requirements:

- **Processor Architecture:** Your system should be x86-64. To check, run:
  ```bash
  uname -a
  ```
  The output should mention `x86_64`.

- **Memory:** At least 4GB of RAM is recommended. Verify using:
  ```bash
  free -h
  ```
  Alternatively, if you have `htop` installed:
  ```bash
  htop
  ```
  Press `F10` to exit the interactive display. If not installed, you can install it:
  ```bash
  sudo apt install htop
  ```

- **Administrative Access:** Ensure you have sudo privileges:
  ```bash
  sudo -v
  ```
  A successful command means you have the required access.

## Installing Docker

### Step 1 – Set Up the Docker Repository

To install Docker for the first time, add the official Docker `apt` repository:

1. **Update Your Package List and Install Prerequisites:**
   ```bash
   sudo apt-get update
   sudo apt-get install ca-certificates curl
   ```

2. **Create the Keyrings Directory:**
   ```bash
   sudo install -m 0755 -d /etc/apt/keyrings
   ```

3. **Download and Add Docker’s GPG Key:**
   ```bash
   sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
   ```

4. **Set Correct Permissions on the GPG Key:**
   ```bash
   sudo chmod a+r /etc/apt/keyrings/docker.asc
   ```

5. **Add the Docker Repository and Update:**
   ```bash
   echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   sudo apt-get update
   ```

### Step 2 – Download and Install Docker Desktop

1. **Download the Docker Desktop .deb Package:**  
   Visit [Docker Desktop for Linux](https://docs.docker.com/desktop/install/ubuntu/) and follow the sidebar navigation to download the DEB package (commonly named `docker-desktop-amd64.deb`). This file is typically saved in your `~/Downloads` directory.

2. **Install the Package:**
   ```bash
   sudo apt-get install ./Downloads/docker-desktop-amd64.deb
   ```

   *Tip:* You might see an error like:
   ```
   N: Download is performed unsandboxed as root, as file '/home/user/Downloads/docker-desktop.deb' couldn't be accessed by user '_apt'. - pkgAcquire::Run (13: Permission denied)
   ```
   This warning can be safely ignored.

3. **Verify Docker Installation:**
   ```bash
   docker --version
   ```
   You should see something like:
   ```
   Docker version XX.X.X, build XXXX
   ```

4. **Launching Docker:**
   - **Via Terminal:**
     ```bash
     systemctl --user start docker-desktop
     ```
   - **Via GUI:** Click the Docker tray icon (if using a GNOME environment; if not installed, add it with `sudo apt install gnome-terminal`).

## Installing AlmaLinux 9 in a Docker Container

### Step 1 – Download the AlmaLinux 9 Image

Open a terminal inside Docker (from the Docker Desktop window or your own terminal) and pull the AlmaLinux 9 image:
```bash
docker pull almalinux:9
```
The image will appear in the “Images” section of the Docker Desktop sidebar.

### Step 2 – Create and Run a Container

Create a new container from the AlmaLinux 9 image:
```bash
docker run --name almaLinux_container -it almalinux:9 /bin/bash
```
This command does the following:
- `--name almaLinux_container` assigns a name to your container.
- `-it` starts an interactive session.
- `/bin/bash` opens a Bash shell inside the container.

Once inside, verify the operating system:
```bash
cat /etc/os-release
```
You should see details similar to:
```
NAME="AlmaLinux"
VERSION="9.5 (Teal Serval)"
...
```

### Additional Container Management Tips

- **Container Exit Behavior:**  
  When running a container interactively with `-it`, exiting the shell (via typing `exit` or pressing `Ctrl+D`) will stop the container. If you prefer to keep the container running, detach from it by pressing:
  ```plaintext
  Ctrl+P followed by Ctrl+Q
  ```
  
- **Restarting a Stopped Container:**  
  If the container has stopped, you can restart it and attach interactively:
  ```bash
  docker start -ai almaLinux_container
  ```

- **Auto-Removal Option:**  
  For temporary sessions, run the container with the `--rm` flag so that it is automatically removed once stopped:
  ```bash
  docker run --rm -it almalinux:9 /bin/bash
  ```

- **Viewing Containers:**  
  To list all containers (running and stopped), use:
  ```bash
  docker ps -a
  ```

---

Congratulations—you now have Docker installed on Ubuntu and can run an AlmaLinux 9 container! Enjoy exploring your new environment.

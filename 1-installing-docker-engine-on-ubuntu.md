# Installing Docker Engine on Ubuntu

## 1. **Prerequisites:**

**OS requirements:** Ensure you have the 64-bit variant of Ubuntu to install Docker Engine.

**Uninstall old versions:** Prioritize uninstalling any pre-existing versions before initiating the installation of a new one.

```bash
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

## 2. **Installation using the apt repository**

It's essential to establish the Docker repository before initiating the Docker Engine installation on a new host machine. Afterward, you can install and update Docker from the repository. 

-  Begin by updating the `apt` package index. Following that, install the necessary packages to facilitate `apt` usage of a repository via HTTPS:

```bash
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
```

- Add Docker’s official GPG key:

```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

- Use the following command to set up the repository:

```bash
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

## 2.1. **Install Docker Engine**

- Update the `apt` package index to refresh the apt cache:

```bash
sudo apt-get update
```

- Receiving a GPG error when running `apt-get update`?

```bash
sudo chmod a+r /etc/apt/keyrings/docker.gpg
sudo apt-get update
```

- Selecting the docker repository as the default one

```bash
apt-cache policy docker-ce
```

- Install Docker Engine, containerd, and Docker Compose.

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## 3. **Install using the convenience script**

Docker provides a convenience script at [https://get.docker.com/](https://get.docker.com/) to install Docker into development environments non-interactively. The convenience script isn’t recommended for production environments, but it’s useful for creating a provisioning script tailored to your needs. 

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh --dry-run
```

- This example downloads the script from https://get.docker.com/ and runs it to install the latest stable release of Docker on Linux:

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

- You have now successfully installed and started Docker Engine. The docker service starts automatically on Debian based distributions. On RPM based distributions, such as CentOS, Fedora, RHEL or SLES, you need to start it manually using the appropriate systemctl or service command. As the message indicates, non-root users can’t run Docker commands by default.


## 4. **Manage Docker as a non-root user**

- The Docker daemon binds to a Unix socket, not a TCP port. By default it’s the root user that owns the Unix socket, and other users can only access it using `sudo`. The Docker daemon always runs as the `root` user.

- If you prefer not to prefix sudo before Docker commands, establish a Unix group named docker and add desired users. When the Docker daemon initializes, it generates a Unix socket, accessible to docker group members. Note that in certain Linux distributions, installation of Docker Engine via a package manager might automatically create this group, eliminating the need for manual setup.

- Create the `docker` group and add current user `david` to the Docker group to be able to run the docker command.

```bash
sudo groupadd docker && sudo usermod -aG docker $USER
```

- Log out and log back in so that your group membership is re-evaluated or restart the virtual machine for changes to take effect.

- Verify that you can run docker commands without sudo and remove the image from local Docker image repository.

```bash
 docker run --rm hello-world && docker rmi hello-world
```

## 5. **Configure Docker to Start on Boot with `systemd`**

Many modern Linux distributions use `systemd` to manage which services start when the system boots. On  `Debian` and `Ubuntu,` the `Docker` service starts on boot by default.

- To automatically start `Docker` and `containerd` on boot for other Linux distributions using systemd, run the following command:

```bash
 sudo systemctl enable docker.service && sudo systemctl enable containerd.service
```

- Log out and then in for a change to take effect.

```bash
exit
```

- After logging in, check the docker version to check if the docker client can talk to the docker daemon or server.

```bash
docker --version
```

### Reference: [https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/)

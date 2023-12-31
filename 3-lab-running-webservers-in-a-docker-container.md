# Lab: Running Web Servers in a Docker Container

## 1. Running Web Servers in a Docker Container

- Running three nginx and one Apache web servers

```bash
docker container run -d -p 80:80 --name mysite1 nginx
docker container run -d -p 8080:80 --name mysite2 nginx
docker container run -d -p 8081:80 --name mysite3 nginx
docker container run -d -p 8082:80 --name mysite4 httpd
```

- Verify that the web servers, nginx, are running.

```bash
docker container ls
```

or

```bash
docker ps
```


![Untitled](assets/images/Untitled.png)

- Enter the `ipaddress:port` of the server into a web browser to confirm its status.

![Untitled](assets/images/lab-running-a-webserver-in-a-docker-container/Untitled%201.png)

![Untitled](assets/images/lab-running-a-webserver-in-a-docker-container/Untitled%202.png)

![Untitled](assets/images/lab-running-a-webserver-in-a-docker-container/Untitled%203.png)

![Untitled](assets/images/lab-running-a-webserver-in-a-docker-container/Untitled%204.png)

## 2. Stopping a container

- use docker container stop `CONTAINER_ID|CONTAINER_NAME.` For example, the container_id of a running nginx web server is `a5bd16af0a4b`, and the container_name is `mysite3`

```bash
docker container stop a5bd16af0a4b
```

- or use the command below to kill a container by name.

```bash
docker container stop mysite2
```

![Untitled](assets/images/lab-running-a-webserver-in-a-docker-container/Untitled.png)

- stopping multiple containers using the first three characters of the `container_id`

```bash
docker container stop a5b e36 faa 568
```

## 3. Remove a Running Container and Docker Image from Local

Example: removing a container image, `nginx`, and `hello-world` with running container_names: `mysite1 mysite2 mysite3 mysite4 mysite5` requires force `-f` removing the running containers before the images can be removed.

```bash
docker remove rm mysite1 mysite2 mysite3 mysite4 mysite5 -f
docker image rm nginx httpd hello-world
```

## 4. Removing Dangling Docker Images

Docker images are composed of multiple layers. Dangling images refer to layers that no longer have a connection to tagged images. To optimize system resources, it's advisable to delete these dangling images, along with stopped containers and networks not associated with at least one container. Execute the following commands to perform these actions:

```bash
docker system prune -y
```

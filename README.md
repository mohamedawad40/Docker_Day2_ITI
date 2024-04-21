# ITI - Docker Lab2 🐋

## Task 1:
Run a container using nginx image, and mount a directory from your host into the 
Docker container. example: /home/samy/nginx:/home/nginx (bind mount)

#### 1-create a directory and file iti.txt in it 
```bash
mkdir nginx_bindMount
cd nginx_bindMount
touch iti.txt
```
#### 2- Run container using nginx image
```bash
docker run -d --name nginx_bindMount -v /root/nginx_bindMount:/usr/share/nginx/html nginx
```

#### 3-check if the file is mounted on the machine
```bash
docker exec -it nginx_bindMount bash
cd /usr/share/nginx/html/
ls
```

---

## Task 2:
### Steps
#### 1. Create 2 docker network (net-1 & net-2)
```bash
docker network create net1
docker network create net2
```
#### 2. Run 2 new containers using nginx:alpine image, and attach the net-1 to them
```bash
docker run -d --name nginx-1 nginx:alpine
docker run -d --name nginx-2 nginx:alpine

docker network connect net-1 nginx-1
docker network connect net-1 nginx-2
```
#### 3.  Run another 1 new containers using nginx:alpine image, and attach the net-2 to them
```bash
docker run -d --name nginx-3 nginx:alpine
docker network connect net-2 nginx-3
```
#### 4. Inspect the 3 containers to know their IPs and write them aside
```bash
docker inspect -f '{{.NetworkSettings.Networks.net1.IPAddress}}' nginx-1
172.18.0.2
docker inspect -f '{{.NetworkSettings.Networks.net1.IPAddress}}' nginx-2
172.18.0.3
docker inspect -f '{{.NetworkSettings.Networks.net2.IPAddress}}' nginx-3
172.20.0.2
```
#### 5. Enter a container in the net-1 network and try to ping a container in the net-2 
network (What do you notice?)
```bash
docker exec -t nginx-1 bash
ping 172.20.0.02
the ping fails because the two containers are in different networks.
```
#### 6. Enter a container in the net-1 network and try to ping the other container in the 
same network (What do you notice?)
```bash
docker exec -t nginx-1 bash
ping 172.18.0.03
the ping is successful because the two containers are in the same networks.
```
---
## Task 3: Explain the difference between Docker volumes and Bind Mount.
```bash
Docker Volume:

Volumes are managed by Docker and stored within the Docker environment,It is stored within a directory on the Docker host,
Managed by Docker and are isolated from the core functionality of the host machine.
Docker volumes can be named and managed independently of containers , Can be mounted into multiple containers simultaneously,
Deleting a container does not delete the volume

Bind Mount:

Bind mounts are linked to a specific directory or file on the Docker host filesystem.
The file or directory is referenced by its full path on the host machine.
When you use a bind mount, a file or directory on the host machine is mounted into a container.
```



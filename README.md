## Deploy nexus as docker container from nexus image

### Technologies used:

Docker, Nexus, DigitalOcean, Linux

### Project Description:

1- Create and Configure Droplet

2- Create a docker named volumes

2- Set up and run Nexus as a Docker container from docker official nexus image

### With Docker

Step 1: Create and configure a Droplet server on DigitalOcean with firewall attached

######Create a Droplet server on Sydney region with SSH method authentication

![Alt text](./image/Screenshot%202023-02-08%20at%207.29.29%20pm.png?raw=true)
######Configure the Droplet server with firewall attached at port 22 for SSH

![Alt text](./image/Screenshot%202023-02-08%20at%207.29.02%20pm.png?raw=true)

Step 2: Login Droplet server with SSH method

```
ssh root@170.64.186.65
```

![Alt text](./image/login-server-ssh-root.png?raw=true)

Step 3: Install and configure Docker

###### Update apt and install Docker

```
update apt
```

![Alt text](./image/Screenshot%202023-02-08%20at%207.27.33%20pm.png?raw=true)

```
apt install docker.io
```

```
docker --version
```

![Alt text](./image/Screenshot%202023-02-08%20at%207.32.42%20pm.png?raw=true)
Step 4: Create a docker named volume for nexus container to persist data

```
docker volume create --name nexus-data
```

![Alt text](./image/Screenshot%202023-02-08%20at%207.37.14%20pm.png?raw=true)

Step 5: Create and run Nexus container

######Pull nexus3 image:latest from Docker hub

```
docker pull sonatype/nexus3
```

######Run nexus container from nexus3 image with named volume

```
docker run -d -p 8081:8081 --name nexus -v nexus-data:/nexus-data sonatype/nexus3
```

![Alt text](./image/Screenshot%202023-02-08%20at%207.38.22%20pm.png?raw=true)

######Check currently running process on Droplet server and port number

```
netstat -lnpt
```

![Alt text](./image/Screenshot%202023-02-08%20at%207.43.35%20pm.png?raw=true)

Step 6: Update the firewall on Droplet server with listening port
![Alt text](./image/Screenshot%202023-02-08%20at%207.46.54%20pm.png?raw=true)
######Open the nexus application via 170.64.186.65:8081 in the browser
![Alt text](./image/Screenshot%202023-02-08%20at%207.46.54%20pm.png?raw=true)

Step 7: Browser the nexus container data

######Enter the nexus container session

```
docker exec -it a29e4487ca97 /bin/bash
```

######Nexus container create a non-root user: nexus to start the nexus application automatically

```
whoami
```

![Alt text](./image/Screenshot%202023-02-08%20at%207.51.24%20pm.png?raw=true)
######Check the referenced named volume in nexus container

```
ls /
```

```
ls /nexus-data/
```

![Alt text](./image/Screenshot%202023-02-08%20at%208.16.20%20pm.png?raw=true)
######Exit from nexus container session

```
exit
```

Step 8: Browser the docker host machine server data
######Check the created named volume

```
docker volume ls
```

######Inspect the created named volume

```
docker exec -it a29e4487ca97 /bin/bash
```

![Alt text](./image/Screenshot%202023-02-08%20at%207.55.01%20pm.png?raw=true)
######Check the named volume in docker host machine server

```
ls /var/lib/docker/volumes/nexus-data/_data
```

![Alt text](./image/Screenshot%202023-02-08%20at%208.15.29%20pm.png?raw=true)

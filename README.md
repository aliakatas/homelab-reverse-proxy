# Homelab Reverse Proxy
Deploying **Traefik** as a container is the best approach, especially since the services are already containerized. 
This keeps everything modular, easier to manage, and makes updates simpler.  

This setup assumes:

✅ **Docker + Docker Compose**  
✅ **automatic HTTPS with Let's Encrypt**  
✅ **dynamic routing based on container labels**  

---

### **Step 1: Create a Docker Network**
First, create a **dedicated network** for Traefik to communicate with other containers:  
```bash
docker network create traefik-net
```
This keeps your setup isolated and allows easy service discovery.

---

### **Step 2: Create a `docker-compose.yml` for Traefik**
Prepare Traefik's setup by editing (or accepting as-is) the [`docker-compose.yml](./docker-compose.yml) file.

This setup:
- **Uses the latest Traefik v3** container
- **Mounts the Docker socket** (allows automatic container discovery as long as they are using labels)
- **Exposes necessary ports** HTTPS (443), and the dashboard (8080)
- **Uses a dedicated network** to connect with your other services

If needed, create a folder for `letsencrypt`:
```sh
sudo mkdir /mnt/letsencrypt
```
Also, consider changing ownership of the newly created folder.

---

### **Step 3: Create the `traefik.yml` Configuration File**
Prepare (or accept as-is) the [`traefik.yml`](./traefik.yml) file.

This setup:
- Enables **HTTP (80) and HTTPS (443)**
- **Auto-discovers containers** using Docker labels
- Uses **Let's Encrypt** for automatic SSL  
- Exposes the **Traefik dashboard** on port **8080**  

---

### **Step 4: Deploy and Verify**
Run:
```bash
docker compose up -d
```
Check logs:
```bash
docker logs -f traefik
```
Visit the dashboard at **`http://your-server-ip:8080`**.

---

### **Step 5: Deploy sample services for testing(e.g., Whoami)**
To test Traefik, the main [`docker-compose.yml`](./docker-compose.yml) contains an apache server. 
Let's test with another container with a simple **Whoami** service.
Edit (or accept as-is) the [test docker-compose](./test-service-1/docker-compose.yml).

Deploy it:
```bash
docker compose up -d
```
Then visit **`https://whoami.yourdomain.com`**.

---



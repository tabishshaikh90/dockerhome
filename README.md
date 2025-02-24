Docker Setup with Traefik Reverse Proxy & Self-Signed SSL
This repository provides a Docker-based self-hosted infrastructure using Traefik as a reverse proxy, self-signed SSL certificates, and services like Pi-hole, Uptime Kuma, Nextcloud, Jellyfin, Heimdall, phpMyAdmin, MariaDB, and Portainer.

🔹 Key Features:

Traefik Reverse Proxy with self-signed SSL certificates for secure local access.
Pi-hole for ad-blocking and DNS resolution.
Uptime Kuma for monitoring uptime and services.
Nextcloud as a private cloud solution.
Jellyfin as a self-hosted media server.
Heimdall for a centralized service dashboard.
Portainer for managing Docker containers easily.
MariaDB & phpMyAdmin for database management.

🚀 Installation Steps

1️⃣ Install Docker & Docker Compose
Run the following commands to install Docker and Docker Compose on Ubuntu/Debian:

sudo apt update && sudo apt upgrade -y
sudo apt install -y docker.io docker-compose
sudo systemctl enable --now docker

For CentOS/RHEL:

sudo yum install -y docker
sudo systemctl enable --now docker
Verify installation:

docker --version
docker-compose --version
2️⃣ Install Portainer (Optional)
Portainer provides a GUI for managing Docker containers. Install it with:

docker volume create portainer_data

docker run -d --name=portainer --restart=always \
  -p 9443:9443 -p 8000:8000 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce:latest

Access Portainer UI at:
➡️ https://<your-server-ip>:9443

3️⃣ Clone This Repository
git clone https://github.com/yourusername/docker-traefik-setup.git
cd docker-traefik-setup

4️⃣ Generate Self-Signed SSL Certificate
Before running Traefik, generate a self-signed SSL certificate:

mkdir -p certs
openssl req -x509 -newkey rsa:2048 -keyout certs/key.pem -out certs/cert.pem -days 365 -nodes -subj "/CN=192.168.0.119"

5️⃣ Update /etc/hosts (For Local Domain Resolution)
To access services via friendly domain names (pihole.local, uptime.local, etc.), add entries to /etc/hosts:

sudo nano /etc/hosts

Your_Server_IP traefik.local pihole.local uptime.local nextcloud.local phpmyadmin.local jellyfin.local heimdall.local

Save and exit.

6️⃣ Start All Services

docker-compose up -d
This will:

Start Traefik with self-signed SSL.
Deploy Pi-hole, Uptime Kuma, Nextcloud, Jellyfin, Heimdall, phpMyAdmin, MariaDB, and Portainer.
Enable automatic HTTPS for all services.

🌍 Access Services
Service	URL
Traefik Dashboard	https://traefik.local:8080
Pi-hole	https://pihole.local/admin/
Uptime Kuma	https://uptime.local
Nextcloud	https://nextcloud.local
phpMyAdmin	https://phpmyadmin.local
Jellyfin	https://jellyfin.local
Heimdall	https://heimdall.local
Portainer	https://192.168.0.119:9443
✅ Manage & Restart Services
Check running containers:

docker ps
Restart a specific service:

docker-compose restart <service-name>
Stop all services:

docker-compose down
Start all services again:

docker-compose up -d
🛠️ Troubleshooting
If a service is not accessible: 1️⃣ Check Logs:

docker logs traefik --tail 50
docker logs pihole --tail 50
docker logs uptime-kuma --tail 50
2️⃣ Restart Traefik:

docker-compose restart traefik
3️⃣ Check DNS Resolution:

ping pihole.local
4️⃣ Ensure /etc/hosts is updated.

📢 Contribute
✅ Found a bug? Submit an issue.
✅ Want to improve it? Fork and send a PR!

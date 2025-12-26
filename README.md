# MERN_Travel_Memory

Project URL : https://sneha.store/
<img width="897" height="358" alt="Screenshot 2025-12-14 at 10 32 59 PM" src="https://github.com/user-attachments/assets/4c6b7051-d220-496a-967b-6f53c7fa7d37" />

Deployment Steps
1. EC2 Instance Setup
- Created an Ubuntu-based EC2 instance
- Assigned a static Elastic IP: 50.18.111.84
- Opened required ports in the security group:
22 (SSH)
80 (HTTP)
443 (HTTPS)

<img width="1101" height="693" alt="Screenshot 2025-12-14 at 10 33 40 PM" src="https://github.com/user-attachments/assets/ad46bbd5-d06d-4411-ba69-67b656dbe58f" />

2. Server Configuration
   # update
sudo apt update && sudo apt upgrade -y

# essential packages
sudo apt install -y git nginx build-essential curl

# Node.js (recommended: install Node 18 LTS or Node 20)
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs

# PM2 process manager (global)
sudo npm install -g pm2

# static serve tool for frontend (choose one)
sudo npm install -g serve http-server

#  certbot for letsencrypt
sudo apt install -y certbot python3-certbot-nginx

---

### 3. Clone the Repository


git clone https://github.com/UnpredictablePrashant/TravelMemory
cd TravelMemory

<img width="415" height="180" alt="image" src="https://github.com/user-attachments/assets/b3c98d56-cfe1-46e9-9526-847b0366ce1d" />



<img width="415" height="384" alt="image" src="https://github.com/user-attachments/assets/e2710861-7455-489f-b856-74b96097f1b3" />



4. MongoDB Atlas Configuration
Created a MongoDB Atlas cluster
Allowed EC2 IP access in Network Access
Generated MongoDB connection string

<img width="415" height="225" alt="image" src="https://github.com/user-attachments/assets/0eb61948-7264-459b-bf32-d749c2215d5c" />

Backend .env file:

PORT=3000
MONGO_URI=mongodb+srv://<username>:<password>@cluster.mongodb.net/travelmemory

5. Backend Configuration
Configured backend to run multiple instances using PM2
Created ecosystem.backend.config.js
Backend instances run on:
Port 3001
Port 3002

ecosystem.backend.config.js:

module.exports = {
  apps: [
    {
      name: "travel-frontend-1",
      cwd: "/home/ubuntu/TravelMemory/frontend",
      script: "npx",
      args: "serve -s build -l 8080"
    },
    {
      name: "travel-frontend-2",
      cwd: "/home/ubuntu/TravelMemory/frontend",
      script: "npx",
      args: "serve -s build -l 8081"
    }
  ]
}



6. Frontend Configuration
Updated frontend environment variable to use nginx as proxy
Frontend .env:
REACT_APP_BACKEND_URL=/api

<img width="960" height="744" alt="image" src="https://github.com/user-attachments/assets/4a413d8c-72aa-4c8f-a65f-81126ea6ef22" />

Build frontend:
npm run build
ecosystem.frontend.config.js:
module.exports = {
apps: [
{
name: "travel-frontend-1", cwd: "/home/ubuntu/TravelMemory/frontend", script: "npx", args: "serve -s build -l 8080"
},{
name: "travel-frontend-2", cwd: "/home/ubuntu/TravelMemory/frontend", script: "npx", args: "serve -s build -l 8081"
}
]}
8. nginx Reverse Proxy and Load Balancing
   Created nginx configuration file:

   sudo nano /etc/nginx/sites-available/travelmemory

nginx configuration (/etc/nginx/sites-available/travelmemory):
upstream travel_backend {
    # simple round-robin
    server 127.0.0.1:3001;
    server 127.0.0.1:3002;
}

upstream travel_frontend {
    server 127.0.0.1:8080;
    server 127.0.0.1:8081;
}

server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;

    # Redirect requests with '/api' to backend
    location /api/ {
        proxy_pass http://travel_backend;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
    }

    # Serve static frontend via load-balanced upstream
    location / {
        proxy_pass http://travel_frontend;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
    }

    # (Optional) Increase body size if uploading images
    client_max_body_size 50M;
}

<img width="1338" height="240" alt="image" src="https://github.com/user-attachments/assets/da540617-ccd2-4527-a1b2-2f935aa46de8" />

Frontend served via nginx / PM2.
<img width="2468" height="608" alt="image" src="https://github.com/user-attachments/assets/4c0d8b35-7b5c-4432-a1e8-ef91ec753f39" />

8. Domain Configuration
Purchased a custom domain
Updated DNS records in domain provider:
A Record: Root domain → EC2 Elastic IP
CNAME Record: www → root domain




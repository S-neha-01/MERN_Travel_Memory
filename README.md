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

```bash
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






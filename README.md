# 🚗 RidePlatform — Production-Grade AWS Infrastructure

A production-ready ride-booking platform backend built on AWS.  
**Designed to demonstrate real-world AWS engineering skills.**

---

## 🏗️ Architecture

```
Users → Route 53 → CloudFront → ALB → EC2 (Auto Scaling) → RDS MySQL
                                           ↓
                                          S3 (assets + backups)
                                           ↓
                                     CloudWatch + SNS (monitoring)
```

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Runtime | Node.js 18 + Express.js |
| Database | MySQL 8 (AWS RDS Multi-AZ) |
| Web Server | Nginx (reverse proxy) |
| Container | Docker |
| IaC | Terraform |
| CI/CD | Jenkins + GitHub |
| Monitoring | CloudWatch + Grafana |
| Storage | AWS S3 |

---

## 🚀 Run Locally (Ubuntu)

### Step 1 — Install prerequisites
```bash
# Install Docker
sudo apt-get update
sudo apt-get install -y docker.io docker-compose
sudo usermod -aG docker $USER
newgrp docker
```

### Step 2 — Clone the project
```bash
git clone https://github.com/YOUR_USERNAME/aws-ride-platform.git
cd aws-ride-platform
```

### Step 3 — Start everything
```bash
docker-compose up --build
```

### Step 4 — Test the API
```bash
# Health check
curl http://localhost/health

# Register a rider
curl -X POST http://localhost/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"name":"Mahesh","email":"mahesh@test.com","password":"pass123","phone":"9999999999","role":"rider"}'

# Login
curl -X POST http://localhost/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"mahesh@test.com","password":"pass123"}'

# Request a ride (use token from login response)
curl -X POST http://localhost/api/rides/request \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{"pickup_lat":18.5204,"pickup_lng":73.8567,"dropoff_lat":18.5314,"dropoff_lng":73.8446,"pickup_address":"Pune Station","dropoff_address":"FC Road"}'
```

---

## 📡 API Endpoints

| Method | Endpoint | Description | Auth |
|---|---|---|---|
| GET | /health | ALB health check | No |
| POST | /api/auth/register | Register user | No |
| POST | /api/auth/login | Login | No |
| POST | /api/rides/request | Request a ride | Yes |
| GET | /api/rides | My ride history | Yes |
| GET | /api/rides/:id | Ride details | Yes |
| PATCH | /api/rides/:id/status | Update ride status | Yes |
| GET | /api/rides/available/list | Available rides (drivers) | Yes |
| GET | /api/drivers/profile | Driver profile | Yes |
| PATCH | /api/drivers/location | Update location | Yes |
| PATCH | /api/drivers/availability | Online/Offline | Yes |

---

## ☁️ AWS Deployment

See `terraform/` folder for complete infrastructure code.

**Quick deploy:**
```bash
cd terraform
terraform init
terraform plan
terraform apply
```

---

## 📊 Monitoring

- CloudWatch Dashboard: CPU, memory, ALB requests, RDS connections
- Alarms: CPU > 80%, RDS storage < 10GB, 5xx errors > 10/min
- Logs: CloudWatch Log Groups (30-day retention)

---

## 👤 Author
**Mahesh Jadhav** — AWS Cloud & Network Engineer  
📧 krishnagyan7822@gmail.com | 📱 7822821943

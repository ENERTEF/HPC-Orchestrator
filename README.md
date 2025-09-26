# HPC-Orchestrator v0.1

Repository for the HPC Orchestrator (master–agent) developed under **WP3/T3.3**.

---

## ✨ Features
The first implementation supports:
- Installing the **manager** and one **agent** on a central VM  
- Installing an **agent** on a VM to be monitored  

---

## ⚙️ Setup HPC Manager

1. Clone the contents of `/src/manager`.
2. Setup prometheus.yml, for every agent you have installed there should be a tuple where IP is the public IP of the agent VM
   ```yaml
   - job_name: 'job name'
     static_configs:
       - targets: ["$IP:9100"]
3. Bring up Prometheus manager `docker-compose up -d --build`
4. This will setup HPC manager and an agent at your local VM

## ⚙️Setup HPC agent
- Clone contents of /src/agent
- ```docker compose up -d --build```
- agent exposes metrics at `$IP:9100`

---
## Deploy Procedure
- Deploy HPC manager on a central VM
- Deploy agent on the VM to monitor
- Bring down manager `docker-compose down`
- Edit `prometheus.yml` on manager VM:
   - To add an agent, add the public IP of the slave VM:
   ```yaml
   - job_name: 'nodeSlave'
     static_configs:
       - targets: ["$Slave_IP:9100"]
- Rerun manager `docker-compose up -d`

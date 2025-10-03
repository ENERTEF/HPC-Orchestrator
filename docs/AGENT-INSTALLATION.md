# 📘 Agent Installation Guide (Prometheus Node Exporter)

This document describes how to install and run the monitoring agent (**Prometheus Node Exporter**) on your TEF so it can be monitored by our central Prometheus system.

---

## 1. Purpose
The Node Exporter collects system metrics (CPU, memory, disk, network) and makes them available to our central Prometheus monitoring server.

---

## 2. System Requirements
- **Operating System:** Linux (Ubuntu 20.04+, Debian 10+, CentOS 7+, or similar)
- **CPU/RAM:** Minimal (runs in background, <50MB memory usage)
- **Network Requirements:**
  - **Agent → Manager:** The agent VM must be able to reach the Prometheus manager (`enertef-server.eurodyn.com` on **TCP/9090**) for health checks and future integrations.  
    To test this, the following commands should work:
    ```bash
    ping enertef-server.eurodyn.com
    curl enertef-server.eurodyn.com:9090
    ```
  - **Manager → Agent:** The manager VM must be able to reach the TEF by a public IP or domain name. Ensure that is feasible.  
  - In case of failure, we recommend enabling firewall rules that allow communication **only between the manager and your agent**.

- **Software:**
  - Docker ≥ 20.x
  - Docker Compose ≥ 1.29

---

## 3. Installation Steps

1. **Create a working directory**
   ```bash
   mkdir -p ~/node_exporter && cd ~/node_exporter
   ```

2. **Create `docker-compose.yml`**  
   You can either create the file manually or download it from [GitHub](https://github.com/ENERTEF/HPC-Orchestrator/blob/main/src/agent/docker-compose.yml):

   ```yaml
   version: '3.8'
   services:
     node-exporter:
       image: prom/node-exporter:v1.9.1
       container_name: node-exporter
       ports:
         - "9100:9100"
       restart: unless-stopped
   ```

3. **Start the agent**
   ```bash
   docker-compose up -d
   ```

---

## 4. Verification
1. From your server, check that metrics are being served:
   ```bash
   curl http://localhost:9100/metrics
   ```

2. Contact **European Dynamics** to update the HPC manager accordingly and monitor your VM.

---

## 5. Security Notes
- Do **not** expose port `9100` publicly on the internet. Restrict access to the Prometheus manager IP only.  
- Ensure **bidirectional network connectivity** between the agent VM and the manager VM.  
- The container auto-restarts on reboots, so no manual intervention is needed.  

---

## 6. Support
If you face issues during installation, please contact:  

**konstantinos.touloumis@eurodyn.com**  
Subject: *"EnerTEF: TEF HPC agent installation"*  

Please include:  
- Server IP  
- OS version  
- Output of:  
  ```bash
  docker logs node-exporter
  ```

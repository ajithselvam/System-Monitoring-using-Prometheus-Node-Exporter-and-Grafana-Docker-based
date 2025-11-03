# System-Monitoring-using-Prometheus-Node-Exporter-and-Grafana-Docker-based

Monitored macOS system metrics using Prometheus and Node Exporter, visualized through Grafana dashboards — all deployed via Docker containers.


# 🖥️ System Monitoring using Prometheus, Node Exporter, and Grafana (Docker-based)

Monitor and visualize system-level metrics (CPU, memory, disk, and network usage) from your macOS machine using **Prometheus**, **Node Exporter**, and **Grafana**, all running in Docker containers.

---

## 🚀 Overview

This project demonstrates how to:
- Export system metrics from macOS using **Node Exporter**
- Collect and store metrics using **Prometheus**
- Visualize the metrics using **Grafana**
- Connect all services via a Docker bridge network — without docker-compose

---

## 🧰 Prerequisites

- macOS with **Docker Desktop** installed
- Terminal access
- Basic understanding of Docker commands

---

## ⚙️ Step 1: Create a Project Folder

```bash
mkdir prometheus-grafana
cd prometheus-grafana


📝 Step 2: Create the Prometheus Configuration File


nano prometheus.yml

global:
  scrape_interval: 5s

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]

  - job_name: "node_exporter"
    static_configs:
      - targets: ["node-exporter:9100"]


Save and close (Ctrl + O, Enter, Ctrl + X).



🌐 Step 3: Create a Docker Network
docker network create monitor-net


Step 4: Run Prometheus
docker run -d \
  --name prometheus \
  -p 9090:9090 \
  --network monitor-net \
  -v $(pwd)/prometheus.yml:/etc/prometheus/prometheus.yml \
  prom/prometheus


✅ This:

Runs Prometheus on port 9090

Mounts your custom prometheus.yml file

Starts collecting metrics from Node Exporter


Step 5: Run Node Exporter
docker run -d \
  --name node-exporter \
  -p 9100:9100 \
  --network monitor-net \
  prom/node-exporter


✅ This container exposes your macOS system metrics (CPU, memory, disk, etc.) on port 9100.




Step 6: Run Grafana
docker run -d \
  --name grafana \
  -p 3000:3000 \
  --network monitor-net \
  grafana/grafana


✅ Grafana runs on http://localhost:3000




Default credentials:

Username: admin
Password: admin



Step 7: Verify the Setup
1️⃣ Check Prometheus

Visit http://localhost:9090/targets

You should see both:

prometheus — UP

node_exporter — UP

2️⃣ Connect Prometheus to Grafana

Open Grafana → http://localhost:3000

Login → Connections → Data Sources → Add Data Source

Select Prometheus

Set URL to:

http://host.docker.internal:9090


Click Save & Test → should say “Data source is working!”

3️⃣ Import a Dashboard

In Grafana, go to + → Import

Enter Dashboard ID: 1860

Choose your Prometheus data source

Click Import

✅ You’ll now see live system metrics including:

CPU Usage

Memory Consumption

Disk Space

Network Traffic


📘 Understanding the Volume Mount

The flag -v $(pwd)/prometheus.yml:/etc/prometheus/prometheus.yml means:

Part	Meaning
-v	Mounts a volume between host and container
$(pwd)	Refers to your current local folder path
/etc/prometheus/prometheus.yml	The path inside the Prometheus container where it expects its config


🧾 Summary

You’ve successfully set up a monitoring stack with:

Tool	Purpose	Port
Prometheus	Data collection & storage	9090
Node Exporter	Exports system metrics	9100
Grafana	Visualization dashboard	3000

👤 Author

Ajith Selvam N
Network Engineer @ Experience.com
📍 macOS | Docker | Prometheus | Grafana | Node Exporter

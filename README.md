# **Dockerized Monitoring Stack with Prometheus & Grafana for NVIDIA GPU Metrics**

This repository provides a **Docker Compose setup** for monitoring NVIDIA GPU metrics using **Prometheus** and **Grafana**. It collects GPU telemetry via `nvidia_gpu_exporter`, which must be installed on the host machine. The Prometheus service scrapes metrics from `localhost:9835/metrics`, and Grafana provides a pre-configured dashboard for visualization.

## **Features**
- 🚀 **Prometheus**: Collects GPU telemetry from `nvidia_gpu_exporter`.
- 📊 **Grafana**: Displays real-time metrics with a preloaded dashboard.
- 🔄 **Docker Compose**: Simplifies setup and deployment.

---

## **Installation & Setup**

### **1. Install `nvidia_gpu_exporter` on the Host**
Follow the [installation guide](https://github.com/utkuozdemir/nvidia_gpu_exporter/blob/master/INSTALL.md):

> All credits to utkuozdemir for his contributions to the community.

Verify `nvidia_gpu_exporter` running:

```sh
curl http://localhost:9835/metrics
```

---

### **2. Clone the Repository**
```sh
git clone https://github.com/darwincastro/NVIDIA_Exporter_Grafana_Prometheus.git
cd NVIDIA_Exporter_Grafana_Prometheus
```

---

### **3. Start the Monitoring Stack**
```sh
docker-compose up -d
```
This will start:
- **Prometheus** (listening on port `9090`)
- **Grafana** (accessible on port `3000`)

---

### **4. Access the Monitoring Tools**
- **Grafana**: Open `http://localhost:3000/`
  - Default login: `admin / admin`
  - A **pre-configured dashboard** is available.
- **Prometheus**: Open `http://localhost:9090/`
  - Verify target status at `Status → Targets`.

---

## **Troubleshooting**
- **Metrics not appearing in Grafana?**
  - Check if Prometheus can reach the exporter:
    ```sh
    docker-compose logs prometheus
    ```
  - Ensure the `nvidia_gpu_exporter` service is running.

**Verify the connection:**

- Check if Prometheus is scraping the nvidia_gpu_exporter by going to http://<docker_host>:9090/targets.

- **Prometheus shows exporter as `DOWN`?**
  - Edit `prometheus.yml`:
    ```yaml
    scrape_configs:
      - job_name: 'nvidia_gpu_exporter'
        static_configs:
          - targets: ['host.docker.internal:9835']
    ```
  - Restart:
    ```sh
    docker-compose down && docker-compose up -d
    ```

---

## **License**
This project is open-source and released under the **MIT License**.

---

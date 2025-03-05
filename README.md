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

```sh
wget https://github.com/utkuozdemir/nvidia_gpu_exporter/releases/latest/download/nvidia_gpu_exporter-linux-amd64
chmod +x nvidia_gpu_exporter-linux-amd64
sudo mv nvidia_gpu_exporter-linux-amd64 /usr/local/bin/nvidia_gpu_exporter
```

Then, **start the exporter**:

```sh
nvidia_gpu_exporter --web.listen-address=":9835" &
```

Verify it's running:

```sh
curl http://localhost:9835/metrics
```

---

### **2. Clone the Repository**
```sh
git clone https://github.com/YOUR_GITHUB_USERNAME/YOUR_REPO_NAME.git
cd YOUR_REPO_NAME
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

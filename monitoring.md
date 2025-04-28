
# Monitoring and Debugging Production Servers with Prometheus and Grafana

---

## 1. What is Monitoring?

Monitoring is the process of **collecting**, **analyzing**, and **visualizing** system metrics like CPU usage, memory usage, network traffic, app errors, etc., to ensure your application/server stays healthy.

---

## 2. What is Prometheus?

- **Prometheus** is an open-source **monitoring and alerting tool**.
- It **collects** metrics data from applications and servers.
- It **stores** the data and allows you to **query** it using a language called **PromQL**.

---

## 3. What is Grafana?

- **Grafana** is an open-source **visualization** tool.
- It **connects to Prometheus** (or other databases) and creates beautiful **dashboards** and **graphs** from the metrics.

---

## 4. Setup: Installing Prometheus and Grafana

### Option 1: Install on Local Machine (Linux/Mac)

#### Install Prometheus

1. Download and extract Prometheus:
```bash
# Download latest Prometheus
wget https://github.com/prometheus/prometheus/releases/download/v2.48.1/prometheus-2.48.1.linux-amd64.tar.gz

# Extract
tar xvfz prometheus-2.48.1.linux-amd64.tar.gz

# Move into directory
cd prometheus-2.48.1.linux-amd64
```

2. Start Prometheus server:
```bash
./prometheus --config.file=prometheus.yml
```

> Prometheus will start at: [http://localhost:9090](http://localhost:9090)

---

#### Install Grafana

1. Install Grafana using APT (Debian/Ubuntu):
```bash
# Install dependencies
sudo apt-get install -y adduser libfontconfig1

# Download Grafana
wget https://dl.grafana.com/oss/release/grafana_10.1.2_amd64.deb

# Install Grafana
sudo dpkg -i grafana_10.1.2_amd64.deb
```

2. Start Grafana:
```bash
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

> Grafana will start at: [http://localhost:3000](http://localhost:3000)

**Default Login:**
- Username: `admin`
- Password: `admin` (you’ll be asked to change)

---

### Option 2: Install Using Docker

If you prefer Docker:

**Run Prometheus:**
```bash
docker run -d --name=prometheus -p 9090:9090 prom/prometheus
```

**Run Grafana:**
```bash
docker run -d --name=grafana -p 3000:3000 grafana/grafana
```

---

## 5. Configure Prometheus

**Edit `prometheus.yml` to tell Prometheus what to monitor.**

Example `prometheus.yml`:
```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']
```

If you have an application exposing metrics, add it under `static_configs`.

---

## 6. Configure Grafana to Use Prometheus

1. Open Grafana at [http://localhost:3000](http://localhost:3000).
2. Go to **Configuration > Data Sources > Add data source**.
3. Select **Prometheus**.
4. Set URL to:
```
http://localhost:9090
```
5. Click **Save & Test**.

Now Grafana can pull data from Prometheus!

---

## 7. Create Dashboards in Grafana

1. Go to **Create > Dashboard > Add a new panel**.
2. Choose a metric (like `up` or `node_cpu_seconds_total`).
3. Customize the graph, alerting rules, etc.
4. Save the dashboard.

---

## 8. Monitoring and Debugging Tips

- **Monitor CPU usage**:
```PromQL
rate(node_cpu_seconds_total{mode!="idle"}[5m])
```
- **Monitor Memory usage**:
```PromQL
node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes
```
- **Monitor Disk usage**:
```PromQL
node_filesystem_avail_bytes
```
- **Set Alerts**:
  - Go to **Alert Rules** in Grafana.
  - Create a rule like "CPU > 90% for 5 minutes" to trigger alerts.

---

## 9. Quick Important Commands

| Command | Description |
|:--------|:------------|
| `./prometheus --config.file=prometheus.yml` | Start Prometheus manually |
| `sudo systemctl start grafana-server` | Start Grafana service |
| `docker run -d --name=prometheus -p 9090:9090 prom/prometheus` | Run Prometheus in Docker |
| `docker run -d --name=grafana -p 3000:3000 grafana/grafana` | Run Grafana in Docker |
| `kubectl port-forward svc/prometheus 9090:9090` | Access Prometheus in Kubernetes |
| `kubectl port-forward svc/grafana 3000:3000` | Access Grafana in Kubernetes |

---

## 🎯 Key Takeaways

- **Prometheus** = collects and stores metrics.
- **Grafana** = visualizes and analyzes metrics.
- **Monitoring production servers** helps you **catch problems early** before customers notice.
- Use **PromQL** to create powerful custom queries!

---

# ✅ You're Now Ready to Monitor Like a Pro!
```
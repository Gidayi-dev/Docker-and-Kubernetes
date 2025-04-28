# Monitoring and Debugging Production Servers with Prometheus and Grafana

---

## 1. What is Monitoring?

Monitoring is the process of **collecting**, **analyzing**, and **visualizing** metrics (data) to detect problems in real time, understand system behavior, and optimize performance.

---

## 2. What is Prometheus?

- **Prometheus** is an open-source **metrics collection and alerting tool**.
- It **pulls metrics** from configured targets at given intervals.
- It stores the metrics and allows querying with **PromQL** (Prometheus Query Language).

---

## 3. What is Grafana?

- **Grafana** is an open-source **dashboard tool** that **connects** to databases like Prometheus.
- It helps create **visualizations** (graphs, charts, alerts) from the collected metrics.

---

## 4. Setup: Installing Prometheus and Grafana

### Option 1: Install Directly (Linux/Mac)

#### Install Prometheus

```bash
# Download Prometheus
wget https://github.com/prometheus/prometheus/releases/download/v2.48.1/prometheus-2.48.1.linux-amd64.tar.gz

# Extract the archive
tar xvfz prometheus-2.48.1.linux-amd64.tar.gz

# Enter Prometheus directory
cd prometheus-2.48.1.linux-amd64
```

#### Run Prometheus

```bash
./prometheus --config.file=prometheus.yml
```

**Access Prometheus:** [http://localhost:9090](http://localhost:9090)

---

#### Install Grafana

```bash
# Install dependencies
sudo apt-get install -y adduser libfontconfig1

# Download Grafana
wget https://dl.grafana.com/oss/release/grafana_10.1.2_amd64.deb

# Install the package
sudo dpkg -i grafana_10.1.2_amd64.deb
```

#### Run Grafana

```bash
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

**Access Grafana:** [http://localhost:3000](http://localhost:3000)  
**Default login:**  
- Username: `admin`
- Password: `admin` (you'll be asked to change)

---

### Option 2: Install Using Docker

If you prefer Docker, much faster:

```bash
# Run Prometheus
docker run -d --name=prometheus -p 9090:9090 prom/prometheus

# Run Grafana
docker run -d --name=grafana -p 3000:3000 grafana/grafana
```

---

## 5. Configure Prometheus

Edit the `prometheus.yml` file to tell Prometheus **what to scrape** (monitor).

Example minimal `prometheus.yml`:

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']
```

If you have an app exposing metrics (like a Node app, or a server), add it like:

```yaml
  - job_name: 'myapp'
    static_configs:
      - targets: ['localhost:8000']
```

Restart Prometheus after changes!

---

## 6. Configure Grafana to Use Prometheus

1. Open Grafana: [http://localhost:3000](http://localhost:3000)
2. Login with `admin / admin`.
3. Click **"Add your first data source"**.
4. Choose **Prometheus**.
5. Set the URL to `http://localhost:9090`.
6. Save and test the connection.

🎉 Now Grafana is pulling data from Prometheus!

---

## 7. Create Your First Dashboard in Grafana

1. Click **"+" → Dashboard → New Panel**.
2. Click **"Query"** and select your Prometheus data source.
3. Type a simple query like:

```promql
up
```
This shows if your targets are **up or down**.

4. Change the visualization type (Graph, Gauge, Table, etc.)
5. Save the dashboard.

---

## 8. Common Prometheus Queries (PromQL)

| Query | Purpose |
|:------|:--------|
| `up` | Show all targets that are up (1) or down (0) |
| `node_cpu_seconds_total` | CPU usage metrics |
| `node_memory_MemAvailable_bytes` | Memory available |
| `http_requests_total` | Total HTTP requests (if instrumented) |
| `rate(http_requests_total[5m])` | HTTP request rate over the last 5 mins |

---

## 9. Debugging with Prometheus and Grafana

### Prometheus Side
- Check if Prometheus is scraping targets successfully (`Status → Targets`).
- Use queries to find high CPU, memory leaks, dropped targets.

### Grafana Side
- Use **alerts**: Grafana can send an alert if a query value exceeds a threshold (like CPU > 80%).
- Create **dashboards** that clearly visualize trends (memory usage rising? traffic spikes?).

---

## 10. Useful Commands

| Command | Description |
|:--------|:------------|
| `docker ps` | List running containers |
| `docker stop container_name` | Stop a container |
| `systemctl status grafana-server` | Check Grafana server status |
| `systemctl status prometheus` | Check Prometheus service status |
| `curl localhost:9090/metrics` | See raw Prometheus metrics output |

---

## 🎯 Quick Tips

- **Always** monitor `up` query first to ensure targets are healthy.
- **Create alerts** for critical metrics (like CPU, Memory, Disk Space).
- **Use Docker** if you want fast installation without cluttering your system.
- **Dashboards + Alerts** = real production monitoring.

---

# 🚀 Key Takeaways

- **Prometheus** collects metrics (data).
- **Grafana** shows the data (beautiful graphs).
- Together, they give you full **insight** into the health and performance of your servers and apps.

---
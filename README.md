# Firedancer Monitoring with Prometheus & Grafana

Minimal Docker Compose setup to monitor Firedancer via Prometheus and Grafana.

This setup provides:
- A Prometheus instance scraping Firedancer metrics
- A Grafana instance with dashboards to visualize Firedancer performance

---

## 📦 Prerequisites
- Docker & Docker Compose installed
- A Firedancer node exposing Prometheus metrics on 0.0.0.0:7999
- Linux host with ufw firewall rules managed (see below)

---

## 🧱 Directory Structure

```
project-root/
├── docker-compose.yml
├── prometheus.yml
├── grafana/
│   └── provisioning/
│       ├── datasources/
│       │   └── prometheus.yml
│       └── dashboards/
│           ├── dashboard.yml
│           ├── quic.json
│           ├── tile_details.json
│           └── tile_overview.json
```

---

## ⚙️ Configuration Files

### docker-compose.yml
Contains the Prometheus and Grafana services configuration.

### prometheus.yml
Configures Prometheus to scrape Firedancer metrics from `host.docker.internal:7999`.

### grafana/provisioning/datasources/prometheus.yml
Automatically configures Prometheus as the default datasource in Grafana.

### grafana/provisioning/dashboards/
Contains official Firedancer dashboards from the [Firedancer repository](https://github.com/firedancer-io/firedancer/tree/main/contrib/grafana):
- **quic.json**: QUIC protocol metrics and performance
- **tile_details.json**: Detailed tile-level metrics and performance
- **tile_overview.json**: Overview of all tiles and their status

---

## 🔥 Firedancer Configuration

Add the following section to your Firedancer configuration TOML file to enable Prometheus metrics and restart your process:

```toml
[metrics]
    prometheus_listen_address = "0.0.0.0:7999"
```

---

## 🔐 Firewall (ufw)

Allow Docker containers to reach the host:

```bash
sudo ufw allow from 172.16.0.0/12 to any port 7999 proto tcp  # Docker bridge range
```

---

## 🚀 Usage

```bash
docker compose up -d
```

Access:
- Prometheus: http://localhost:9090
- Grafana: http://localhost:3000 (admin / admin)

**Note**: Prometheus is automatically configured as the default datasource in Grafana, and official Firedancer dashboards are automatically provisioned.

---

## 📊 Dashboards

The following official Firedancer dashboards are automatically provisioned:

- **QUIC Dashboard**: Monitor QUIC protocol performance and metrics
- **Tile Details**: Detailed view of individual tile performance
- **Tile Overview**: High-level overview of all tiles and their status

These dashboards are sourced from the [official Firedancer repository](https://github.com/firedancer-io/firedancer/tree/main/contrib/grafana) and will be automatically available in the "Firedancer" folder in Grafana.

---

## 📌 Notes
- Prometheus talks to Firedancer via host.docker.internal + host-gateway.
- Grafana runs as root to avoid volume permission issues. You can restrict this if you mount volumes with correct ownership.
- If your Firedancer exposes on another port, update `prometheus.yml` and firewall rules.
- Prometheus datasource is automatically configured in Grafana via provisioning.
- Official Firedancer dashboards are automatically provisioned and ready to use.

---

## ✅ TODO (optional improvements)
- Set up alerting for leader performance/backpressure
- Secure Grafana with basic auth or OAuth
- Add custom dashboards for specific use cases

---

## 📞 Questions

Feel free to open an issue or ping me if you need help adapting this to a specific deployment.
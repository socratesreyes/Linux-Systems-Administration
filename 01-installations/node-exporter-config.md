# Node Exporter Installation and Configuration

Install Prometheus Node Exporter on CentOS Stream 9 to collect hardware and OS metrics, and configure it as a systemd service.

Prerequisites
- **OS**: CentOS Stream release 9
- **Access**: Root or sudo privileges
- **Network**: Port `9100` open on the local firewall

## 🚀 The Lab Setup

### 1. Configure the Firewall
Allow incoming traffic on port 9100 through firewalld:
```bash
sudo firewall-cmd --permanent --add-port=9100/tcp
sudo firewall-cmd --reload
```

### 2. Create a System User
Create a dedicated, non-login system user for security isolation:
```bash
sudo useradd --no-create-home --shell /bin/false node_exporter
```

### 3. Download and Extract Node Exporter
Download the latest stable Linux binary and extract it:
```bash
cd /tmp
curl -LO https://github.com
tar -xvf node_exporter-1.8.2.linux-amd64.tar.gz
```

### 4. Move Binary to System Path
Move the executable to standard binary storage and update ownership:
```bash
sudo mv node_exporter-1.8.2.linux-amd64/node_exporter /usr/local/bin/
sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
```

### 5. Create Systemd Service File (`/etc/systemd/system/node_exporter.service`)
Create the service unit file to manage the lifecycle of Node Exporter:
```ini
[Unit]
Description=Prometheus Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
```

### 6. Enable and Start the Service
Reload systemd configurations, enable the service on boot, and start it immediately:
```bash
sudo systemctl daemon-reload
sudo systemctl enable --now node_exporter
```

## 🔍 Verification

### 1. Check Service Status
Confirm the service is active and running successfully under systemd:
```bash
sudo systemctl status node_exporter --no-pager
```

### 2. Query Local Metrics Endpoint
Verify that Node Exporter is emitting metrics locally via curl:
```bash
curl http://localhost:9100/metrics
```

### 3. Prometheus Scrape Target Configuration
Add this snippet to your main `prometheus.yml` file under `scrape_configs` to start pulling data from this node:
```yaml
  - job_name: 'centos-node'
    static_configs:
      - targets: ['<CENTOS_SERVER_IP>:9100']
```

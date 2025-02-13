- Monitor Linux server with Prometheus and Grafana using node exporter.

##### Prometheus configuration

- prometheus.yml is the configuration file for Prometheus where we can do all changes regarding configuration of Prometheus.
- Prom tool is command-line utility tool is used to verify the configuration of Prometheus.
- Prom QL is query language used by Prometheus internally.

Q. What is Node Exporter ?
- Node exporter is one of the Prometheus exporters which is used to expose servers or system OS metrics.
- With the help of Node exporter we can expose various resources of the system like RAM, CPU Utilization, Memory Utilization, disk space etc.
- Node Exporter runs as a system service which gathers the metrics of your system and that gathered metrics is displayed with the help of Grafana Visualization Tool.

##### Configure Linux Server Monitoring with Prometheus and Grafana Hands On

- create Prometheus System Users and Directory

```
sudo useradd --no-create-home --shell /bin/false prometheus

sudo useradd --no-create-home --shell /bin/false node_exporter

sudo mkdir /etc/prometheus

sudo mkdir /var/lib/prometheus
```

- update Prometheus user

```
sudo chown prometheus:prometheus /etc/prometheus

sudo chown prometheus:prometheus /var/lib/prometheus
```

- Download Prometheus Binary File

```
cd /opt/

wget https://github.com/prometheus/prometheus/releases/download/v2.26.0/prometheus-2.26.0.linux-amd64.tar.gz
```

- Install Prometheus and Grafana on Ubuntu 20.04 LTS
```
sha256sum prometheus-2.26.0.linux-amd64.tar.gz

tar -xvf prometheus-2.26.0.linux-amd64.tar.gz

cd prometheus-2.26.0.linux-amd64

ls - to check list of setup files
```

- copy Prometheus Binary files
```
sudo cp /opt/prometheus-2.26.0.linux-amd64/prometheus /usr/local/bin/

sudo cp /opt/prometheus-2.26.0.linux-amd64/promtool /usr/local/bin/
```

- Update Prometheus user ownership on Binaries
```
sudo chown prometheus:prometheus /usr/local/bin/prometheus

sudo chown prometheus:prometheus /usr/local/bin/promtool
```

- copy Prometheus console libraries
```
sudo cp -r /opt/prometheus-2.26.0.linux-amd64/consoles /etc/prometheus

sudo cp -r /opt/prometheus-2.26.0.linux-amd64/console_libraries /etc/prometheus

sudo cp -r /opt/prometheus-2.26.0.linux-amd64/prometheus.yml /etc/prometheus
```

- Update Prometheus ownership on Directories
```
sudo chown -R prometheus:prometheus /etc/prometheus/consoles

sudo chown -R prometheus:prometheus /etc/prometheus/console_libraries

sudo chown -R prometheus:prometheus /etc/prometheus/prometheus.yml
```

- check Prometheus version
```
prometheus --version

promtool --version
```

- Prometheus configuration file
```
cat /etc/prometheus/prometheus.yml 
```

- creating Prometheus systemd file
```
sudo -u prometheus /usr/local/bin/prometheus \ --config.file /etc/prometheus/prometheus.yml \ --storage.tsdb.path /var/lib/prometheus/ \ --web.console.templates=/etc/prometheus/consoles \ --web.console.libraries=/etc/prometheus/console_libraries

sudo nano /etc/systemd/system/prometheus.service
```

- content of `/etc/systemd/system/prometheus.service`
```
[Unit] 
Description=Prometheus 
Wants=network-online.target 
After=network-online.target 

[Service] 
User=prometheus 
Group=prometheus 
Type=simple 
ExecStart=/usr/local/bin/prometheus \ 
	--config.file /etc/prometheus/prometheus.yml \ 
	--storage.tsdb.path /var/lib/prometheus/ \ 
	--web.console.templates=/etc/prometheus/consoles \ 
	--web.console.libraries=/etc/prometheus/console_libraries 

[Install] 
WantedBy=multi-user.target
```

```
sudo systemctl daemon-reload

sudo systemctl start prometheus

sudo systemctl enable prometheus

sudo systemctl status prometheus
```

- Accessing Prometheus
```
sudo ufw allow 9090/tcp

UI
http://server-IP-or-Hostname:9090
```

###### Install Grafana on Ubuntu 20.04 LTS using APT
```
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add –

echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list

sudo apt-get update

sudo apt-get install grafana

sudo systemctl start grafana-server

sudo systemctl status grafana-server

sudo systemctl enable grafana-server.service

http://your_ip:3000

Credentials :- admin / admin

```

- Configure Prometheus as Grafana Data Source

###### Install Node Exporter on Ubuntu 20.04 LTS

- Get the latest tar file url from official Prometheus node_exporter site
- install it
```
wget https://github.com/prometheus/node_exporter/releases/download/v1.2.0/node_exporter-1.2.0.linux-amd64.tar.gz

sudo tar xvzf node_exporter-1.2.0.linux-amd64.tar.gz

cd node_exporter-1.2.0.linux-amd64

sudo cp node_exporter /usr/local/bin
```

- Creating Node Exporter Systemd service
```
cd /lib/systemd/system

sudo nano node_exporter.service
```

```
[Unit] 
Description=Node 
Exporter Wants=network-online.target 
After=network-online.target 

[Service] 
Type=simple 
User=node_exporter 
Group=node_exporter 
ExecStart=/usr/local/bin/node_exporter \ 
		-— collector.mountstats \ 
		-— collector.logind \ 
		-— collector.processes \ 
		-— collector.ntp \ 
		-— collector.systemd \ 
		-— collector.tcpstat \ 
		-— collector.wifi 
Restart=always 
RestartSec=10s 

[Install] 
WantedBy=multi-user.target
```

```
sudo systemctl daemon-reload

sudo systemctl enable node_exporter

sudo systemctl start node_exporter

sudo systemctl status node_exporter
```

- Configure the Node Exporter as a Prometheus target

```
cd /etc/prometheus

sudo nano prometheus.yml

add below configuration in static_configs:
- targets: ['localhost:9090', 'localhost:9100']

sudo systemctl restart prometheus

https://localhost:9100/targets
```

- Creating Grafana Dashboard to Monitor Linux Server
```
Import the dashboard with ID 14513, it will load all the graphas for it.
```



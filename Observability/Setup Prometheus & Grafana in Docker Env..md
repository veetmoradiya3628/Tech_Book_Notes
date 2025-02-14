- Monitor docker containers with Prometheus and Grafana

###### Run Grafana using Docker container

```
docker run -d -p 3000:3000 --name=grafana grafana/grafana
```

- Visit `http://your_ip_address:3000/login` to access grafana UI, credentials for the same is admin / admin.

##### Run Prometheus using Docker container

- create Prometheus configuration directory

```
sudo mkdir prometheus

sudo nano prometheus.yml
```

- content for prometheus.yml is as follow,
```
scrape_configs:
  - job_name: 'prometheus'
    scrape_interval: 5s
    static_configs:
      - targets: ['YOUR_SERVER_IP:9090']
```

- Dockerfile for Prometheus `sudo nano Dockerfile`

```
FROM prom/prometheus
ADD prometheus.yml /etc/prometheus/
```

- build docker image & run container out of it.

```
docker build -t my-prometheus .

docker run -p 9090:9090 my-prometheus

# Access prometheus UI with below url,
https://Your_IP_Address:9090
```

###### Integrate the Docker containers with Grafana

```
sudo nano /etc/docker/daemon.json

# content
{
        "metrics-addr" : "0.0.0.0:9323",
        "experimental" : true
}
```

For apply this change you have to restart docker service **`sudo systemctl restart docker`**
- now check the logs on - `http://<public-ip>:9323/metrics`
###### Add new target in prometheus.yml located in prometheus docker container

```
docker exec -it <prometheus-container-id> /bin/sh

cd /etc/prometheus

vi  prometheus.yml

```

- Add Docker job in prometheus.yaml file.

```
- job_name: 'docker'
scrape_interval: 5s
static_configs:
- targets: ['<Your _IP>:9323']
```

- restart the container, `docker restart <container id>`
- Now open Grafana UI and configure data source for Prometheus and generate dashboard from available scraped metrics.

###### Reference
- https://www.fosstechnix.com/monitor-docker-containers-with-prometheus/


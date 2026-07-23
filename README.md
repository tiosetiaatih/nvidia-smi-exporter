# Nvidia SMI Exporter

Dockerized Prometheus exporter for GPU statistics from [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface), written in Go.
Supports multiple GPUs.

# Supported tags and respective `Dockerfile` links

- [`cuda12`, `latest`](docker/cuda12/Dockerfile)
  - based on [nvidia/cuda:12.6.2-base-ubuntu24.04](https://hub.docker.com/r/nvidia/cuda/tags?name=12.6.2-base-ubuntu24.04)
- [`cuda11`](docker/cuda11/Dockerfile)
  - based on [nvidia/cuda:11.5.2-base-ubuntu20.04](https://hub.docker.com/r/nvidia/cuda/tags?name=11.5.2-base-ubuntu20.04)

# How to use

Run with a Docker command:
```
docker run --privileged --runtime nvidia -p 9202:9202/tcp e7db/prometheus-nvidiasmi:cuda12
```

Run through a docker-compose file:
```
services:
  prometheus-nvidiasmi:
    image: e7db/prometheus-nvidiasmi:cuda12
    privileged: true
    runtime: nvidia
    ports:
      - "9202:9202/tcp"
```

Check result at: [http://localhost:9202/metrics](http://localhost:9202/metrics)

Important:
- Don't forget to add `privileged` and `runtime: nvidia` to the Docker Compose file.
- Install package nvidia toolkit first

```bash
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
&& curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list

sudo apt update
sudo apt install -y nvidia-container-toolkit
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker
```

# Grafana dashboard

[Nvidia SMI Metrics dashboard](https://grafana.com/grafana/dashboards/12357) on Grafana Labs

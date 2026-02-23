## Setting up Prometheus on AWS

1. Launch and Ubuntu EC2 instance for Prometheus: \
System configuration 
    - t3.medium
    - 20gb disk space (general-purpose SSD) 
    - Security Group: Allow inbound access on ports: 9090 for Prometheus Web UI, 
    - 22 for SSH access
        - ```ssh -i your-key.pem ubuntu@your-ec2-public-ip```

2. Update packages: ```sudo apt update && sudo apt upgrade -y```

3. Download Prometheus
```wget https://github.com/prometheus/prometheus/releases/download/v2.46.0/prometheus-2.46.0.linux-amd64.tar.gz``` \
```tar -xvzf prometheus-2.46.0.linux-amd64.tar.gz``` \
```mv prometheus-2.46.0.linux-amd64 prometheus```

4. Move files to standard paths:
```sudo mv prometheus /etc/prometheus``` \
```sudo mv /etc/prometheus/prometheus /usr/local/bin/```

5. Create Prometheus Configuration:
- Open the file for editing: ```sudo nano /etc/prometheus/prometheus.yml```
- Edit the File:

```YAML
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: "flask-app"
    static_configs:
      - targets: ["a6bf6255d5f61470c9782b8955c98271-1409247973.us-east-1.elb.amazonaws.com:5000"]  # Replace with your app's External IP
```

5.1 Save the File: ctrl+o -> enter -> ctrl+x \
5.2 Verify the Changes: ```cat /etc/prometheus/prometheus.yml```

6. Locate the Prometheus Binary(Run the following command to find where the prometheus executable is installed):
which prometheus
This should return the full path to the prometheus binary, such as ```/usr/local/bin/Prometheus```

7. Run Prometheus with the config file:
```/usr/local/bin/prometheus --config.file=/etc/prometheus/prometheus.yml```

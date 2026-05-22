# Quick Start — Build Your First AirVibe Node

This guide helps you create your first basic AirVibe environmental monitoring station using Raspberry Pi.

The goal is to provide a simple and reproducible starting point for:
- schools
- students
- citizen science projects
- environmental monitoring
- educational workshops
- community deployments

---

# What You Need

## Hardware

Recommended:
- Raspberry Pi Zero 2 W
- Raspberry Pi 3/4/5

Additional components:
- microSD card (16 GB or larger)
- power supply
- Wi-Fi connection
- environmental sensors

Possible sensors:
- Enviro+
- PMS5003
- TSL2591
- BME280
- EE08
- future soil sensors

---

# 1. Install Raspberry Pi OS

Download:
https://www.raspberrypi.com/software/

Recommended version:
- Raspberry Pi OS Lite

Flash the OS using:
- Raspberry Pi Imager

Enable:
- SSH
- Wi-Fi
- hostname

---

# 2. First Boot

Connect to your Raspberry Pi:

```bash
ssh pi@your-pi-ip
```

Update system:

```bash
sudo apt update && sudo apt upgrade -y
```

---

# 3. Install Python Environment

Install Python tools:

```bash
sudo apt install python3-pip python3-venv git -y
```

Create virtual environment:

```bash
python3 -m venv airvibe-env
```

Activate environment:

```bash
source airvibe-env/bin/activate
```

---

# 4. Clone AirVibe Repository

```bash
git clone https://github.com/usektogo/airvibe-iot.git
cd airvibe-iot
```

---

# 5. Install Sensor Libraries

Example:

```bash
pip install influxdb-client
pip install pimoroni-bme280
```

Additional libraries depend on your sensor setup.

---

# 6. Configure InfluxDB

AirVibe uses InfluxDB for time-series storage.

Typical configuration includes:
- URL
- token
- organization
- bucket

Example:

```python
INFLUX_URL = "http://your-server:8086"
INFLUX_TOKEN = "your-token"
INFLUX_ORG = "airvibe"
INFLUX_BUCKET = "environment"
```

---

# 7. Run Your First Sensor Script

Example:

```bash
python sensor_script.py
```

Recommended production setup:
- systemd services
- automatic restart
- cache handling
- watchdog recovery

---

# 8. Visualization with Grafana

Grafana allows:
- live dashboards
- historical analysis
- alerts
- public/community dashboards

Future guides will include:
- dashboard templates
- public map integration
- school dashboards
- agricultural monitoring views

---

# Suggested Beginner Path

## Beginner
- Raspberry Pi setup
- single sensor
- local visualization

## Intermediate
- InfluxDB integration
- remote access
- Grafana dashboards

## Advanced
- multiple stations
- public dashboards
- automated deployment
- educational/community infrastructure

---

# Educational Goals

AirVibe is designed to help people learn:
- Linux
- Python
- sensors
- networking
- databases
- visualization
- environmental science

---

# Future Improvements

Planned future additions:
- one-command installer
- school deployment kits
- Docker support
- automated provisioning
- simplified dashboard templates

---

# Community

AirVibe encourages:
- experimentation
- learning
- collaboration
- open environmental monitoring
- citizen science

---

More documentation will be added progressively as the project evolves.
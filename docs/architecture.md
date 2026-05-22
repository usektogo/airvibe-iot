# AirVibe Architecture

## Overview

AirVibe is a distributed environmental monitoring infrastructure built around Raspberry Pi sensor nodes, centralized time-series storage and remote visualization.

The system is designed to be:
- modular
- low-cost
- remotely maintainable
- educational
- scalable
- community-friendly

---

## Core Architecture

```text
Sensors
   ↓
Raspberry Pi station
   ↓
Tailscale secure network
   ↓
Central InfluxDB
   ↓
Grafana dashboards
   ↓
Web visualization / community access
```

---

## Main Components

## 1. Sensor Stations

Each AirVibe node is typically based on:
- Raspberry Pi Zero 2 W
- Raspberry Pi 3/4/5

Possible sensors include:
- Pimoroni Enviro+
- PMS5003
- TSL2591
- EE08
- environmental serial sensors
- future soil and agricultural sensors

Stations can operate:
- indoors
- outdoors
- in schools
- in laboratories
- in municipalities
- in agricultural environments

---

## 2. Raspberry Pi Node Logic

Each node:
- reads sensor data
- formats measurements
- stores temporary cache if offline
- sends data to InfluxDB
- runs as a systemd service
- supports remote maintenance

Many nodes also include:
- watchdog recovery
- network self-healing
- cache resend mechanisms
- clock synchronization protection

---

## 3. Tailscale Network

AirVibe uses Tailscale for:
- secure remote access
- SSH connectivity
- cross-network communication
- simplified deployment

Advantages:
- no complex VPN setup
- secure mesh connectivity
- easy scaling
- remote troubleshooting

---

## 4. InfluxDB Backend

Sensor data is stored in a centralized InfluxDB instance.

The database stores:
- temperature
- humidity
- pressure
- PM measurements
- light measurements
- environmental metadata

Benefits:
- efficient time-series storage
- fast querying
- scalable architecture
- easy Grafana integration

---

## 5. Grafana Visualization

Grafana is used for:
- live dashboards
- historical analysis
- sensor monitoring
- alerting
- public/community visualization

Dashboards can support:
- schools
- municipalities
- researchers
- farmers
- citizen science initiatives

---

## 6. Educational Direction

AirVibe is also intended as:
- a STEM learning platform
- a Raspberry Pi educational ecosystem
- an environmental literacy tool
- an open infrastructure learning project

Students can learn:
- sensors
- Linux
- Python
- networking
- databases
- visualization
- environmental science

---

## 7. Future Expansion

Planned future directions:
- soil monitoring
- greenhouse monitoring
- public live map
- educational deployment kits
- simplified node installer
- open API
- automated provisioning
- AI-assisted environmental analysis

---

## Design Philosophy

AirVibe focuses on:
- accessibility
- openness
- reproducibility
- modularity
- educational value
- environmental awareness

The goal is to create understandable environmental infrastructure that communities can build, maintain and expand themselves.
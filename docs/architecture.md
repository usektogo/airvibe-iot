# AirVibe Architecture

## Overview

AirVibe is a distributed environmental monitoring infrastructure built around Raspberry Pi sensor stations, centralized data storage and public access to environmental information.

The platform combines research, personal and citizen-operated monitoring stations into a shared environmental monitoring network.

The architecture is designed to be:

* modular
* low-cost
* remotely maintainable
* reproducible
* scalable
* educational

---

## Core Architecture

```text
Sensors
   ↓
Raspberry Pi Node
   ↓
Tailscale Network
   ↓
Central InfluxDB (Synology NAS)
   ↓
Grafana
   ↓
Public Access
   ├─ airvibe.info
   └─ future community services
```

Environmental data is collected locally at each monitoring station and transmitted to a centralized backend where it is stored, visualized and made available through public and private services.

---

## Deployment Types

AirVibe currently includes a combination of:

### Research Stations

Stations operated as part of environmental monitoring and research activities.

Examples include:

* EIMV laboratory deployments
* OMS Ljubljana
* other research-oriented installations

### Personal Monitoring Stations

Stations operated by project contributors for development, testing and long-term monitoring.

### Citizen-Operated Stations

Stations operated outside the research environment by individual participants interested in environmental monitoring and open data.

### Pilot Installations

Temporary or experimental deployments used to evaluate locations, sensors or monitoring concepts.

Station locations and deployment types may evolve over time as monitoring priorities and project goals change.

---

## Sensor Stations

Each AirVibe node is typically based on:

* Raspberry Pi Zero 2 W
* Raspberry Pi 3
* Raspberry Pi 4
* Raspberry Pi 5

Common sensor integrations include:

* Pimoroni Enviro+
* PMS5003 particulate sensor
* TSL2591 light sensor
* EE08 temperature and humidity probe
* environmental serial sensors

Future developments may include:

* soil monitoring
* greenhouse monitoring
* agricultural sensors
* microclimate monitoring

Stations can operate:

* indoors
* outdoors
* laboratories
* residential locations
* educational environments
* agricultural environments

---

## Raspberry Pi Node Logic

Each station is responsible for:

* collecting sensor measurements
* preprocessing environmental data
* handling temporary data buffering
* transmitting measurements to InfluxDB
* supporting remote maintenance

Most production nodes operate as automated services and are designed for unattended operation.

Additional features may include:

* watchdog recovery
* automatic restart mechanisms
* cached data retransmission
* network resilience measures

---

## Tailscale Network

AirVibe uses Tailscale as the primary connectivity layer between distributed monitoring stations and central infrastructure.

Tailscale provides:

* secure remote access
* SSH connectivity
* cross-network communication
* simplified deployment and maintenance

This approach allows stations to operate across different networks without requiring complex VPN configuration.

---

## InfluxDB Backend

Environmental data is stored in a centralized InfluxDB instance hosted on a Synology NAS.

The database stores measurements such as:

* temperature
* humidity
* pressure
* particulate matter
* light intensity
* station metadata

Centralized storage enables:

* long-term data retention
* historical analysis
* cross-location comparison
* dashboard visualization
* future data sharing

---

## Grafana Visualization

Grafana is used as the primary visualization platform.

Current dashboards support:

* environmental monitoring
* historical analysis
* station diagnostics
* operational oversight

Future dashboard profiles may support:

* educational deployments
* municipal monitoring
* citizen science initiatives
* agricultural users

---

## Educational Direction

AirVibe also serves as a practical STEAM and environmental learning platform.

The project provides opportunities to learn:

* environmental science
* sensors and instrumentation
* Linux
* Python
* networking
* databases
* data visualization
* open-source technologies

The goal is to make environmental monitoring understandable and accessible while encouraging hands-on learning.

---

## Future Development

Planned future directions include:

* simplified node deployment
* educational deployment kits
* open API access
* community dashboards
* enhanced station status monitoring
* agricultural monitoring modules
* AI-assisted environmental analysis

---

## Design Philosophy

AirVibe focuses on:

* openness
* accessibility
* reproducibility
* practical deployment
* educational value
* environmental awareness

The goal is not only to collect environmental data, but also to create understandable infrastructure that people can learn from, contribute to and expand over time.

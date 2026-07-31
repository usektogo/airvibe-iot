# DOC 8 — Verify Sensors

**Document ID:** DOC 8  
**Category:** 03-Software  
**Version:** 0.1.0  
**Status:** Draft  
**Last Updated:** 2026-07-31  
**Scope:** AirVibe Starter Station  
**Reference Operating System:** Raspberry Pi OS Lite Legacy (Bullseye) 32-bit  

---

# Overview

This document verifies the AirVibe Starter Station hardware after completing DOC 7 — Install Enviro+.

The verification is performed one component at a time so that any failure can be isolated to a specific sensor or subsystem.

---

# Prerequisites

Before starting, ensure that:

- DOC 7 — Install Enviro+ has been completed successfully.
- The Raspberry Pi is connected to the complete AirVibe Starter Station hardware assembly.
- An SSH connection to the Raspberry Pi has been established.
- The Pimoroni virtual environment exists at `~/.virtualenvs/pimoroni`.

---

# Step 1 — Activate and Verify the Python Environment

## Execution Environment

- Shell: Bash
- Virtual Environment: **No**, before running the activation command
- Current Directory: `~/enviroplus-python`

Activate the environment:

```bash
source ~/.virtualenvs/pimoroni/bin/activate
```

The shell prompt should now begin with:

```text
(pimoroni)
```

Verify the active Python interpreter and Enviro+ package:

```bash
which python
python --version
python -c "import enviroplus; print('enviroplus OK')"
```

Verified result:

```text
/home/pi/.virtualenvs/pimoroni/bin/python
Python 3.9.2
enviroplus OK
```

---

# Step 2 — Verify the BME280 Environmental Sensor

## Execution Environment

- Shell: Bash
- Virtual Environment: **Yes — `pimoroni`**
- Current Directory: `~/enviroplus-python`

Run:

```bash
python -c "from bme280 import BME280; bme = BME280(); print('Temperature:', round(bme.get_temperature(),1), '°C'); print('Pressure:', round(bme.get_pressure(),1), 'hPa'); print('Humidity:', round(bme.get_humidity(),1), '%')"
```

Verified example result:

```text
Temperature: 22.5 °C
Pressure: 692.2 hPa
Humidity: 67.2 %
```

The verification is successful when the command returns numeric temperature, pressure, and humidity values without an exception.

A screenshot of the verified BME280 output has been captured for later inclusion.

---

# Step 3 — Verify the LTR559 Light and Proximity Sensor

## Execution Environment

- Shell: Bash
- Virtual Environment: **Yes — `pimoroni`**
- Current Directory: `~/enviroplus-python`

Before running the measurement test, the installed module API was inspected:

```bash
python -c "import ltr559; print([name for name in dir(ltr559) if not name.startswith('_')])"
```

The installed module exposes the `LTR559` class. The measurement command is pending verification.

---

# Verification Progress

- [x] Python virtual environment activated and verified.
- [x] Enviro+ package imported successfully.
- [x] BME280 temperature, pressure, and humidity readings verified.
- [ ] LTR559 light and proximity readings verified.
- [ ] PMS5003 particulate matter readings verified.
- [ ] LCD output verified.

---

# Related Documents

- [DOC 6 — Enable Interfaces](enable-interfaces.md)
- [DOC 7 — Install Enviro+](install-enviro-plus.md)
- [REFERENCE DOC A — Reference Operating System](../00-Reference/reference-os.md)

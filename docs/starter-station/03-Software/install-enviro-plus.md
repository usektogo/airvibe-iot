# DOC 7 — Install Enviro+

**Document ID:** DOC 7  
**Category:** 03-Software  
**Version:** 1.0.0  
**Status:** Released  
**Last Updated:** 2026-07-31  
**Scope:** AirVibe Starter Station  
**Reference Operating System:** Raspberry Pi OS Lite Legacy (Bullseye) 32-bit  
**Estimated Time:** 20–30 minutes  
**Difficulty:** Intermediate

---

# Overview

This document describes how to install the Pimoroni Enviro+ Python software used by the AirVibe Starter Station.

The procedure creates an isolated Python virtual environment, installs the official Enviro+ software, verifies the required Raspberry Pi interfaces after installation, and confirms that the Enviro+ Python package can be imported successfully.

Functional verification of the individual sensors and display is outside the scope of this document and is covered in DOC 8 — Verify Sensors.

---

# Prerequisites

Before starting, ensure that:

- DOC 6 — Enable Interfaces has been completed successfully.
- Raspberry Pi OS Lite Legacy (Bullseye) 32-bit is running.
- An SSH connection to the Raspberry Pi has been established.
- The current user account has `sudo` privileges.
- The Raspberry Pi has Internet access.
- I2C and the hardware serial interface are enabled and verified.

### Important

The commands in this document are executed either inside or outside the Python virtual environment.

Each step explicitly identifies the required virtual-environment state. Do not continue unless the shell prompt matches the state described for that step.

When the virtual environment is active, the prompt begins with:

```text
(pimoroni)
```

---

# Step 1 — Install Required System Packages

## Execution Environment

- Shell: Bash
- Virtual Environment: **No**
- Internet Connection: Required

Install Git and Python virtual-environment support:

```bash
sudo apt install git python3-venv
```

When prompted to continue, enter:

```text
Y
```

Verify Git:

```bash
git --version
```

A Git version number confirms that the installation completed successfully.

---

# Step 2 — Create the Pimoroni Virtual Environment

## Execution Environment

- Shell: Bash
- Virtual Environment: **No**

Create the virtual-environment parent directory:

```bash
mkdir -p ~/.virtualenvs
```

Create the Pimoroni virtual environment:

```bash
python3 -m venv ~/.virtualenvs/pimoroni
```

Verify that the environment was created:

```bash
ls -la ~/.virtualenvs/pimoroni
```

The result should include entries such as:

```text
bin
include
lib
pyvenv.cfg
```

Do not continue unless `~/.virtualenvs/pimoroni/bin/activate` exists.

---

# Step 3 — Activate and Verify the Virtual Environment

## Execution Environment

- Shell: Bash
- Virtual Environment: **No**, before running the activation command

Activate the environment:

```bash
source ~/.virtualenvs/pimoroni/bin/activate
```

The shell prompt should now begin with:

```text
(pimoroni)
```

## Execution Environment After Activation

- Shell: Bash
- Virtual Environment: **Yes — `pimoroni`**

Verify the active Python interpreter:

```bash
which python
python --version
pip --version
```

The `which python` result must be:

```text
/home/pi/.virtualenvs/pimoroni/bin/python
```

The Python and `pip` paths must both refer to the active `pimoroni` environment.

---

# Step 4 — Clone the Official Enviro+ Repository

## Execution Environment

- Shell: Bash
- Virtual Environment: **Yes — `pimoroni`**
- Current Directory: Home directory
- Internet Connection: Required

Clone the official Pimoroni repository:

```bash
git clone https://github.com/pimoroni/enviroplus-python
```

Open the repository:

```bash
cd ~/enviroplus-python
```

Verify the repository state:

```bash
git status
```

The expected result identifies the `main` branch and reports a clean working tree.

---

# Step 5 — Run the Enviro+ Installer

## Execution Environment

- Shell: Bash
- Virtual Environment: **Yes — `pimoroni`**
- Current Directory: `~/enviroplus-python`
- Internet Connection: Required

Run the installer without `sudo`:

```bash
./install.sh
```

### Important

Do not run the complete installer with `sudo`.

The installer uses `sudo` internally only where system-level changes are required. Running the complete script as root is not supported.

During installation, answer the prompts as follows.

## Copy the Examples

When prompted:

```text
Would you like to copy examples to /home/pi/Pimoroni/enviroplus? [y/N]
```

Enter:

```text
Y
```

The examples are copied as part of the verified installation procedure. They are not used for functional verification in this document.

## Install Example Dependencies

When prompted:

```text
Would you like to install example dependencies? [Y/N]
```

Enter:

```text
Y
```

Allow all dependencies to install completely.

## Generate Pimoroni Documentation

When prompted:

```text
Would you like to generate documentation? [y/N]
```

Enter:

```text
N
```

Generating the Pimoroni library documentation is not required for the AirVibe Starter Station reference implementation.

---

# Step 6 — Review the Bullseye Installer Warning

## Execution Environment

- Shell: Bash
- Virtual Environment: **Yes — `pimoroni`**
- Current Directory: `~/enviroplus-python`

On the verified Raspberry Pi OS Bullseye reference platform, the installer may report warnings similar to:

```text
raspi-config: do_serial_cons: not found
raspi-config: do_serial_hw: not found
```

The installer may then finish with:

```text
WARNING: One or more setup commands appear to have failed.
```

These warnings are caused by installer calls to `raspi-config` functions that are not available on the verified Bullseye system.

Do not assume that the required interfaces are incorrectly configured. The interface state must be verified explicitly after the required reboot.

---

# Step 7 — Reboot the Raspberry Pi

## Execution Environment

- Shell: Bash
- Virtual Environment: **Yes — `pimoroni`**

Run:

```bash
sudo reboot
```

The SSH session closes while the Raspberry Pi restarts.

Wait at least **2–3 minutes**, then reconnect through SSH.

### Important

A Python virtual environment is not automatically active after a reboot.

After reconnecting, the prompt must not begin with `(pimoroni)` until the environment is activated again explicitly.

---

# Step 8 — Verify the Interface Configuration

## Execution Environment

- Shell: Bash
- Virtual Environment: **No**

Verify I2C and SPI:

```bash
raspi-config nonint get_i2c
raspi-config nonint get_spi
```

Expected result:

```text
0
0
```

For these `raspi-config` checks, `0` indicates that the interface is enabled.

Verify the required boot configuration:

```bash
grep -E 'enable_uart|dtoverlay=pi3-miniuart-bt|dtoverlay=adau7002-simple' /boot/config.txt
```

Expected entries:

```text
enable_uart=1
dtoverlay=pi3-miniuart-bt
dtoverlay=adau7002-simple
```

Verify the serial and I2C devices:

```bash
ls -l /dev/serial0
ls -l /dev/i2c-1
```

The verification is successful when:

- `/dev/serial0` exists and resolves to a serial device.
- `/dev/i2c-1` exists.

Do not continue if either required device is missing.

---

# Step 9 — Verify the Enviro+ Python Installation

## Execution Environment

- Shell: Bash
- Virtual Environment: **No**, before running the activation command

Activate the environment again:

```bash
source ~/.virtualenvs/pimoroni/bin/activate
```

## Execution Environment After Activation

- Shell: Bash
- Virtual Environment: **Yes — `pimoroni`**

Open the repository:

```bash
cd ~/enviroplus-python
```

Verify that Python is using the correct environment:

```bash
which python
```

Expected result:

```text
/home/pi/.virtualenvs/pimoroni/bin/python
```

Verify that the Enviro+ package can be imported:

```bash
python -c "import enviroplus; print('enviroplus import OK')"
```

Expected result:

```text
enviroplus import OK
```

Verify the installed package version:

```bash
pip show enviroplus
```

The verified installation reports:

```text
Name: enviroplus
Version: 1.0.2
```

---

# Completion Check

Confirm that:

- [ ] Git and `python3-venv` were installed successfully.
- [ ] The virtual environment was created at `~/.virtualenvs/pimoroni`.
- [ ] The virtual environment was activated successfully.
- [ ] `python` and `pip` resolved to the `pimoroni` virtual environment.
- [ ] The official `enviroplus-python` repository was cloned.
- [ ] `./install.sh` was run without `sudo` from the active virtual environment.
- [ ] Installer examples and example dependencies were installed.
- [ ] Pimoroni documentation generation was skipped.
- [ ] The Raspberry Pi rebooted successfully.
- [ ] I2C, SPI, UART, `/dev/serial0`, and `/dev/i2c-1` were verified after reboot.
- [ ] The Enviro+ package imported successfully from the active virtual environment.
- [ ] `enviroplus` version 1.0.2 was reported.

The Enviro+ software is now installed and ready for functional sensor verification.

---

# Related Documents

- [DOC 5 — First Boot](first-boot.md)
- [DOC 6 — Enable Interfaces](enable-interfaces.md)
- [REFERENCE DOC A — Reference Operating System](../00-Reference/reference-os.md)

---

## Next Document

Continue with:

[DOC 8 — Verify Sensors](verify-sensors.md)

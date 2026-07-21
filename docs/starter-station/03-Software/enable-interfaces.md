# DOC 6 — Enable Interfaces

**Document ID:** DOC 6  
**Category:** 03-Software  
**Status:** Released  
**Version:** 1.0.2  
**Last Updated:** 2026-07-21  
**Scope:** AirVibe Starter Station  
**Reference Operating System:** Raspberry Pi OS Lite Legacy (Bullseye) 32-bit  
**Estimated Time:** 5–10 minutes  
**Difficulty:** Beginner

---

# Overview

This document describes how to enable the hardware interfaces required by the AirVibe Starter Station using the Raspberry Pi Software Configuration Tool (`raspi-config`).

The procedure enables the I2C interface and the hardware serial interface (UART), both of which are required by the verified AirVibe Starter Station reference implementation.

---

# Prerequisites

Before starting, ensure that:

- DOC 5 — First Boot has been completed successfully.
- Raspberry Pi OS Lite Legacy (Bullseye) 32-bit is running.
- An SSH connection to the Raspberry Pi has been established.
- The current user account has `sudo` privileges.

---

# Step 1 — Open Raspberry Pi Configuration

<div align="center">
  <img src="images/doc-06-figure-01-raspi-config-main-menu.jpg" alt="Raspberry Pi Software Configuration Tool" width="60%">
</div>

Run:

```bash
sudo raspi-config
```

The Raspberry Pi Software Configuration Tool opens.

Select **Interface Options**.

---

# Step 2 — Open Interface Options

<div align="center">
  <img src="images/doc-06-figure-02-interface-options.jpg" alt="Interface Options" width="60%">
</div>

The AirVibe Starter Station requires the following hardware interfaces:

- I2C
- Serial Port

---

# Step 3 — Enable I2C

<div align="center">
  <img src="images/doc-06-figure-03-enable-i2c.jpg" alt="Enable I2C" width="60%">
</div>

Select **I5 I2C**.

When prompted to enable the ARM I2C interface, select **Yes**.

<div align="center">
  <img src="images/doc-06-figure-04-i2c-enabled.jpg" alt="I2C Enabled" width="60%">
</div>

When the confirmation message appears:

> The ARM I2C interface is enabled.

Select **OK**.

The Raspberry Pi returns to the **Interface Options** menu.

---

# Step 4 — Disable the Serial Login Shell

<div align="center">
  <img src="images/doc-06-figure-05-serial-port-login-shell.jpg" alt="Disable Serial Login Shell" width="60%">
</div>

Select **I6 Serial Port**.

When prompted to allow a login shell over the serial interface, select **No**.

### Important

The AirVibe Starter Station uses the Raspberry Pi hardware serial interface to communicate with the PMS5003 particulate matter sensor.

The serial login shell must be disabled so that the hardware serial interface remains available to the sensor.

---

# Step 5 — Enable the Hardware Serial Interface

<div align="center">
  <img src="images/doc-06-figure-06-enable-serial-port-hardware.jpg" alt="Enable Serial Port Hardware" width="60%">
</div>

When prompted to enable the hardware serial interface, select **Yes**.

<div align="center">
  <img src="images/doc-06-figure-07-serial-port-enabled.jpg" alt="Serial Port Enabled" width="60%">
</div>

Confirm that the result states:

- The serial login shell is disabled.
- The serial interface is enabled.

Select **OK**.

The Raspberry Pi returns to the **Interface Options** menu.

---

# Step 6 — Reboot the Raspberry Pi

<div align="center">
  <img src="images/doc-06-figure-08-reboot-required.jpg" alt="Reboot Required" width="60%">
</div>

Select **Finish** to exit the Raspberry Pi Software Configuration Tool.

When prompted:

> Would you like to reboot now?

Select **Yes**.

The SSH session closes while the Raspberry Pi restarts.

Wait at least **2–3 minutes** before reconnecting. Use PuTTY and the current IP address identified with Advanced IP Scanner, as described in DOC 5.

If the previous IP address no longer responds, scan the network again before reconnecting.

---

# Step 7 — Verify the Hardware Interfaces

After reconnecting through SSH, verify that the required hardware interfaces are available.

## Verify I2C

Run:

```bash
ls /dev/i2c*
```

Expected result:

```text
/dev/i2c-1
```

Additional I2C device entries may also be present, depending on the Raspberry Pi configuration.

The verification is successful when at least one `/dev/i2c-*` device is listed.

## Verify the Hardware Serial Interface

Run:

```bash
ls -l /dev/serial0
```

Example result:

```text
lrwxrwxrwx ... /dev/serial0 -> ttyS0
```

The exact target may differ by Raspberry Pi model and system configuration. The verification is successful when `/dev/serial0` exists and resolves to an available serial device.

Do not continue until both interface checks complete successfully.

---

# Completion Check

Confirm that:

- [ ] I2C was enabled in `raspi-config`.
- [ ] The serial login shell was disabled.
- [ ] The hardware serial interface was enabled.
- [ ] The Raspberry Pi rebooted successfully.
- [ ] The SSH connection was re-established.
- [ ] At least one `/dev/i2c-*` device is available.
- [ ] `/dev/serial0` exists and resolves to a serial device.

The required Raspberry Pi hardware interfaces are now configured and verified.

---

# Related Documents

- [DOC 5 — First Boot](first-boot.md)
- [REFERENCE DOC A — Reference Operating System](../00-Reference/reference-os.md)

---

## Next Document

Continue with:

[DOC 7 — Install Enviro+](install-enviro-plus.md)

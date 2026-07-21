# DOC 5 — First Boot

**Document ID:** DOC 5  
**Category:** 03-Software  
**Status:** Released  
**Version:** 1.0.1  
**Last Updated:** 2026-07-21  
**Scope:** AirVibe Starter Station  
**Reference Operating System:** Raspberry Pi OS Lite Legacy (Bullseye) 32-bit  
**Reference Image:** `2023-05-03-raspios-bullseye-armhf-lite.img.xz`  
**Estimated Time:** 15–30 minutes (depending on updates)  
**Difficulty:** Beginner

---

# Overview

This document describes the first boot procedure for the AirVibe Starter Station reference implementation after the verified AirVibe reference operating system has been written to the microSD card.

The procedure verifies that the Raspberry Pi starts correctly, is reachable over the network using SSH, has Internet connectivity, and is fully updated before continuing with software configuration.

This document is based on the AirVibe reference operating system described in **REFERENCE DOC A — Reference Operating System**.

---

# Prerequisites

Before starting, ensure that:

- The verified AirVibe reference operating system image (`2023-05-03-raspios-bullseye-armhf-lite.img.xz`) has been successfully written to the microSD card.
- SSH has been enabled during OS imaging.
- Network (Wi-Fi or Ethernet) has been configured during OS imaging.
- The microSD card is installed in the Raspberry Pi.
- The Raspberry Pi is connected to power.

---

# Boot the Raspberry Pi

1. Insert the prepared microSD card into the Raspberry Pi.
2. Connect the network cable if using Ethernet.
3. Connect the power supply.
4. Wait for the Raspberry Pi to complete its first boot.

The initial boot may take longer than subsequent startups.

During the first boot, Raspberry Pi OS performs its initial system configuration and applies the settings configured during the imaging process.

Allow several minutes before attempting to connect to the device.

---

# Connect Using SSH

From another computer on the same network, connect to the Raspberry Pi using SSH.

If multicast DNS (mDNS) is available on your network, connect using the configured hostname.

```bash
ssh <username>@<hostname>.local
```

Alternatively, connect using the Raspberry Pi's assigned IP address.

```bash
ssh <username>@<ip-address>
```

If prompted to verify the host fingerprint, type:

```text
yes
```

Then enter the user password when prompted.

After a successful login, the Raspberry Pi shell prompt should appear.

---

# Verify Network Connectivity

Verify that the Raspberry Pi can communicate with the Internet.

```bash
ping -c 4 google.com
```

Successful output should show transmitted and received packets without packet loss.

If the command fails, the cause may be a network connectivity problem or a DNS resolution issue.

Confirm that the Raspberry Pi is connected to the network and that DNS is functioning correctly before continuing.

The command automatically stops after sending four packets.

---

# Verify Hostname

Display the configured hostname.

```bash
hostname
```

The hostname uniquely identifies the Raspberry Pi on the network.

Verify that the displayed hostname matches the expected AirVibe system hostname.

---

# Verify Disk Space

Display filesystem usage.

```bash
df -h
```

This command confirms that the operating system has correctly expanded the filesystem and that sufficient free storage is available for software installation and future updates.

Verify that:

- the root filesystem is mounted correctly
- sufficient free space is available for software installation

---

# Update Package Lists

Refresh the package index.

```bash
sudo apt update
```

This command downloads the latest package information from the configured software repositories.

It does not install updates but ensures the system knows which package versions are currently available.

Allow the command to complete successfully before continuing.

---

# Upgrade Installed Packages

Install all available package updates.

```bash
sudo apt full-upgrade -y
```

This command upgrades installed packages to their latest available versions and resolves package dependency changes required by the update.

The operating system should be fully updated before installing additional software.

This ensures that all subsequent installation steps are performed on the verified AirVibe reference platform.

Depending on the Internet connection and the number of available updates, this process may take several minutes.

Allow the upgrade to complete without interruption.

---

# Remove Unused Packages

Remove packages that are no longer required.

```bash
sudo apt autoremove -y
```

Removing unused packages keeps the operating system clean by deleting software that is no longer required after the upgrade process.

---

# Clean Package Cache

Remove downloaded package files that are no longer required.

```bash
sudo apt clean
```

This command clears the local package cache by removing downloaded installation files that are no longer needed.

Cleaning the cache helps recover disk space without affecting installed software.

---

# Reboot if Required

Some updates require a reboot before they become active.

If a reboot is recommended, restart the Raspberry Pi.

```bash
sudo reboot
```

Wait for the system to restart completely before reconnecting via SSH.

Reconnect using:

```bash
ssh <username>@<hostname>.local
```

or, if you connected using an IP address:

```bash
ssh <username>@<ip-address>
```

---

# Final Verification

After reconnecting, verify that the system is operating normally.

Perform the following checks:

- Confirm that an SSH connection can be established successfully.
- Verify that Internet connectivity is available.
- Verify that the expected hostname is reported.
- Confirm that sufficient disk space remains available.
- Ensure that all package updates completed successfully.
- Verify that no package management errors were reported during the update process.

Completing these checks confirms that the Raspberry Pi is ready for the next stage of software configuration.

---

# Final Verification Checklist

- [ ] Raspberry Pi boots successfully.
- [ ] SSH connection established.
- [ ] Network connectivity verified.
- [ ] Hostname verified.
- [ ] Disk space verified.
- [ ] `sudo apt update` completed successfully.
- [ ] `sudo apt full-upgrade -y` completed successfully.
- [ ] `sudo apt autoremove -y` completed successfully.
- [ ] `sudo apt clean` completed successfully.
- [ ] Raspberry Pi rebooted if required.
- [ ] Final SSH login successful.
- [ ] System ready for further configuration.

---

# Related Documents

- DOC 4 — Operating System Installation (`flash-sd-card.md`)
- REFERENCE DOC A — Reference Operating System

---

## Next Document

Continue with:

`DOC 6 — enable-interfaces.md`

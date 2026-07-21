# DOC 5 — First Boot

**Document ID:** DOC 5  
**Category:** 03-Software  
**Status:** Released  
**Version:** 1.1.0  
**Last Updated:** 2026-07-21  
**Scope:** AirVibe Starter Station  
**Reference Operating System:** Raspberry Pi OS Lite Legacy (Bullseye) 32-bit  
**Reference Image:** `2023-05-03-raspios-bullseye-armhf-lite.img.xz`  
**Reference Hostname:** `aq-off`  
**Reference Network Connection:** Wi-Fi only  
**Reference Discovery Tool:** Advanced IP Scanner  
**Reference SSH Client:** PuTTY  
**Estimated Time:** 20–45 minutes  
**Difficulty:** Beginner

---

# Overview

This document describes the verified first boot procedure for the AirVibe Starter Station.

The procedure covers:

- starting the Raspberry Pi for the first time
- waiting for the initial operating system configuration
- locating the station on the local Wi-Fi network
- confirming that the station is reachable
- connecting through SSH using PuTTY
- verifying and updating the operating system

The AirVibe Starter Station reference implementation uses Wi-Fi only. Ethernet is not part of this procedure.

---

# Prerequisites

Before starting, ensure that:

- The verified AirVibe reference operating system image has been written to the microSD card.
- Hostname `aq-off` was configured in Raspberry Pi Imager.
- The correct Wi-Fi network name and password were configured.
- The correct wireless LAN country was configured.
- SSH was enabled.
- The configured station username and password are available.
- The prepared microSD card is inserted into the Raspberry Pi.
- Advanced IP Scanner is installed on the Windows computer.
- PuTTY is installed on the Windows computer.

---

# Critical Network Requirement

> **Important**
>
> The Raspberry Pi and the Windows computer must be connected to the same local network.
>
> The Raspberry Pi connects through the Wi-Fi network configured in Raspberry Pi Imager. The computer must be connected to that same network before scanning or attempting an SSH connection.

A computer connected to a different Wi-Fi network, guest network, mobile hotspot, or isolated network segment may not be able to discover or reach the Raspberry Pi.

Do not continue until the computer's network connection has been confirmed.

---

# Step 1 — Power On the Raspberry Pi

1. Confirm that the prepared microSD card is fully inserted.
2. Connect the Raspberry Pi power supply.
3. Leave the Raspberry Pi powered on while the initial startup completes.

No monitor, keyboard, mouse, or Ethernet cable is required.

---

# Step 2 — Wait for the First Boot

The first boot takes longer than a normal restart.

During the initial startup, Raspberry Pi OS may:

- expand the filesystem
- apply the configuration created by Raspberry Pi Imager
- initialize the Wi-Fi interface
- connect to the configured wireless network
- start the SSH service

Wait at least **5 minutes** before attempting the first network scan.

On slower microSD cards or Wi-Fi networks, the station may require additional time before it appears.

### Important

Do not disconnect power while the first boot is still in progress.

The Raspberry Pi not appearing immediately in Advanced IP Scanner does not necessarily indicate a failed installation.

---

# Step 3 — Confirm the Computer Is on the Same Network

On the Windows computer, confirm that the active Wi-Fi connection is the same network configured for the Raspberry Pi during the imaging procedure.

Check the Wi-Fi network name shown in Windows and compare it with the SSID entered in Raspberry Pi Imager.

Proceed only when both devices are expected to be on the same local network.

---

# Step 4 — Locate the Raspberry Pi

Open **Advanced IP Scanner**.

Scan the local network and look for the reference hostname:

```text
aq-off
```

Record the IP address shown for `aq-off`.

Example:

```text
192.168.1.120
```

The assigned address will depend on the local network and may change after a restart unless a DHCP reservation is configured.

## If the Hostname Is Not Displayed

The scanner may show the Raspberry Pi without a recognizable hostname.

Look for:

- a newly appearing IP address
- a device identified as Raspberry Pi
- a device manufacturer associated with Raspberry Pi hardware

If necessary, compare scan results before and after powering the Raspberry Pi to identify the new device.

A router or access point DHCP client list may also be used to confirm the assigned IP address.

---

# Step 5 — If the Raspberry Pi Is Not Found

First-boot discovery can be slow and may require more than one scan. This can be especially confusing during an initial installation.

Use the following sequence:

1. Confirm again that the computer is connected to the same Wi-Fi network configured for the Raspberry Pi.
2. Wait another **3–5 minutes**.
3. Run a new scan in Advanced IP Scanner.
4. Check whether `aq-off` or a new IP address appears.
5. If the station is still not visible after sufficient waiting, disconnect the Raspberry Pi power supply.
6. Wait approximately **10 seconds**.
7. Reconnect power.
8. Allow the Raspberry Pi to boot for at least **3–5 minutes**.
9. Scan the network again.

During an initial installation, the Raspberry Pi may become visible only after the second boot and a repeated network scan.

### Important

Do not repeatedly disconnect and reconnect power at short intervals.

Always allow enough time for the operating system to finish booting before deciding that the station is unavailable.

If the station remains unavailable after the second boot, verify the Wi-Fi SSID, password, wireless LAN country, microSD card image, and power supply before repeating the imaging procedure.

---

# Step 6 — Verify Network Reachability

After identifying the IP address, open **Command Prompt** on the Windows computer.

Test the IP address:

```cmd
ping 192.168.1.120
```

Replace `192.168.1.120` with the address reported by Advanced IP Scanner.

A reply confirms that the station is reachable from the computer.

You may also test the hostname:

```cmd
ping aq-off.local
```

### Note

The `.local` hostname depends on multicast DNS support and may not work on every Windows or network configuration.

Failure of `ping aq-off.local` does not prove that the Raspberry Pi is offline. Use the IP address from Advanced IP Scanner as the reference connection method.

Some networks or firewall configurations may also block ping while SSH remains available. Continue with PuTTY when the IP address is known.

---

# Step 7 — Connect Using PuTTY

Open **PuTTY** on the Windows computer.

Configure the session:

```text
Host Name (or IP address): <IP address from Advanced IP Scanner>
Port: 22
Connection type: SSH
```

Example:

```text
Host Name (or IP address): 192.168.1.120
Port: 22
Connection type: SSH
```

Select **Open**.

On the first connection, PuTTY may display a security alert for the server host key.

Confirm the host key only when the IP address matches the Raspberry Pi identified during the network scan.

---

# Step 8 — Log In

When the PuTTY terminal opens, enter the username configured in Raspberry Pi Imager.

Then enter the configured password.

The password is not displayed while typing. This is normal terminal behavior.

A successful login displays the Raspberry Pi shell prompt.

Do not continue until the PuTTY session is open and commands can be entered successfully.

---

# Step 9 — Verify the Hostname

Run:

```bash
hostname
```

Expected result:

```text
aq-off
```

If a different hostname is shown, stop and verify whether the correct microSD card and Raspberry Pi were used.

---

# Step 10 — Verify Internet Connectivity

Run:

```bash
ping -c 4 google.com
```

Successful replies confirm that the Raspberry Pi has network and DNS connectivity.

If the command fails, check the Wi-Fi configuration and Internet access before continuing.

---

# Step 11 — Verify Disk Space

Run:

```bash
df -h
```

Confirm that:

- the root filesystem is mounted correctly
- the filesystem has expanded to use the microSD card
- sufficient free space is available

---

# Step 12 — Update Package Lists

Run:

```bash
sudo apt update
```

Allow the command to complete successfully before continuing.

---

# Step 13 — Upgrade Installed Packages

Run:

```bash
sudo apt full-upgrade -y
```

The upgrade may take several minutes, depending on the Wi-Fi connection and the number of available packages.

Do not interrupt power or close the PuTTY session while the package manager is actively installing updates.

---

# Step 14 — Remove Unused Packages

Run:

```bash
sudo apt autoremove -y
```

---

# Step 15 — Clean the Package Cache

Run:

```bash
sudo apt clean
```

---

# Step 16 — Reboot the Raspberry Pi

Run:

```bash
sudo reboot
```

The PuTTY session will close when the Raspberry Pi restarts.

Wait at least **2–3 minutes** before reconnecting.

Run Advanced IP Scanner again if the previous IP address no longer responds. The router may assign a different address after reboot.

Reconnect through PuTTY using the current IP address.

---

# Completion Check

Confirm that:

- [ ] The Raspberry Pi completed its first boot.
- [ ] The computer and Raspberry Pi were connected to the same local network.
- [ ] `aq-off` or its assigned IP address was located with Advanced IP Scanner.
- [ ] The station was reachable by IP address.
- [ ] A PuTTY SSH session was established successfully.
- [ ] The reported hostname is `aq-off`.
- [ ] Internet connectivity was verified.
- [ ] Disk space was verified.
- [ ] Package update and upgrade commands completed without errors.
- [ ] The Raspberry Pi rebooted successfully.
- [ ] A final PuTTY connection was established after reboot.

The AirVibe Starter Station is now ready for interface configuration.

---

# Related Documents

- [DOC 4 — Operating System Installation](flash-sd-card.md)
- [REFERENCE DOC A — Reference Operating System](../00-Reference/reference-os.md)
- [DOC 6 — Enable Interfaces](enable-interfaces.md)

---

## Next Document

Continue with:

[DOC 6 — Enable Interfaces](enable-interfaces.md)

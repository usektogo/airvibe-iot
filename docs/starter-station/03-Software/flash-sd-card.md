# DOC 4 — Operating System Installation

**Document ID:** DOC 4  
**Category:** 03-Software  
**Status:** Released  
**Version:** 1.0.1  
**Last Updated:** 2026-07-21  
**Scope:** AirVibe Starter Station  
**Reference Operating System:** Raspberry Pi OS Lite Legacy (Bullseye) 32-bit  
**Reference Image:** `2023-05-03-raspios-bullseye-armhf-lite.img.xz`  
**Raspberry Pi Imager Version:** 1.7.5  
**Estimated Time:** 15–30 minutes  
**Difficulty:** Beginner

---

# Overview

This document describes how to prepare a microSD card for the AirVibe Starter Station reference implementation using Raspberry Pi Imager.

The procedure writes the verified AirVibe reference operating system image to the microSD card and configures the settings required for a headless first boot.

The operating system must be installed before the microSD card is inserted into the Raspberry Pi.

This document is based on the AirVibe reference operating system described in **REFERENCE DOC A — Reference Operating System**.

---

# Reference Platform

The AirVibe Starter Station documentation is based on:

- Raspberry Pi OS Lite
- Legacy Bullseye
- 32-bit

Reference image:

`2023-05-03-raspios-bullseye-armhf-lite.img.xz`

This operating system image is used by the verified AirVibe reference implementation.

For information about the operating system selection, supported hardware, image checksum, and engineering rationale, see:

[REFERENCE DOC A — Reference Operating System](../00-Reference/reference-os.md)

---

# Prerequisites

Before starting, ensure that:

- Raspberry Pi Imager is installed on the computer.
- The verified reference image has been downloaded.
- A compatible microSD card is available.
- A microSD card reader is connected to the computer.
- Any required files on the microSD card have been backed up.

### Important

Writing an operating system image permanently erases all existing data on the selected storage device.

Verify the selected storage device carefully before beginning the write process.

---

# Step 1 — Open Raspberry Pi Imager

<div align="center">
  <img src="images/01-raspberry-pi-imager-start.jpg" alt="Raspberry Pi Imager Start" width="60%">
</div>

Open Raspberry Pi Imager.

This procedure was documented using:

```text
Raspberry Pi Imager v1.7.5
```

The interface may differ slightly in other Raspberry Pi Imager versions.

---

# Step 2 — Select the Operating System Image

<div align="center">
  <img src="images/02-select-custom-image.jpg" alt="Select Custom Image" width="60%">
</div>

Select:

**Choose OS → Use custom**

The **Use custom** option allows the verified AirVibe reference operating system image to be selected manually.

---

# Step 3 — Select the Reference Image

<div align="center">
  <img src="images/03-select-bullseye-image.jpg" alt="Select Bullseye Image" width="60%">
</div>

Browse to the downloaded operating system image and select:

```text
2023-05-03-raspios-bullseye-armhf-lite.img.xz
```

This is the operating system image used by the AirVibe Starter Station reference implementation.

### Important

Confirm that the selected filename exactly matches the verified AirVibe reference image.

For information about obtaining and verifying the image, see:

[REFERENCE DOC A — Reference Operating System](../00-Reference/reference-os.md)

---

# Step 4 — Select the Storage Device

<div align="center">
  <img src="images/04-select-storage.jpg" alt="Select Storage" width="60%">
</div>

Insert the microSD card into the card reader.

Select **Choose Storage** and choose the correct microSD card.

### Important

Verify the storage device carefully.

Selecting the wrong device may erase data from another drive connected to the computer.

---

# Step 5 — Verify the Image and Storage Selection

<div align="center">
  <img src="images/05-image-and-storage-selected.jpg" alt="Image and Storage Selected" width="60%">
</div>

Verify that:

- the correct operating system image is selected
- the correct microSD card is selected

Proceed only after both selections have been confirmed.

---

# Step 6 — Open Advanced Options

<div align="center">
  <img src="images/06-open-advanced-options.jpg" alt="Open Advanced Options" width="60%">
</div>

Select the gear icon to open the advanced configuration menu.

The advanced options configure the system before the first boot.

---

# Step 7 — Configure the System

<div align="center">
  <img src="images/07-configure-hostname-ssh-wifi.jpg" alt="Configure Hostname, SSH and Wi-Fi" width="60%">
</div>

Configure the following settings:

- hostname
- username
- password
- wireless LAN
- SSH
- locale
- timezone
- keyboard layout

### Note

Values shown in screenshots are examples only.

Use settings appropriate for the intended AirVibe Starter Station deployment.

Example hostnames include:

```text
aq-off
airvibe-test
starter-station
```

### Hostname Requirements

The hostname should:

- uniquely identify the station on the network
- use lowercase letters where possible
- avoid spaces
- remain consistent throughout the deployment documentation

### SSH Configuration

Enable SSH before writing the operating system image.

SSH access is required because the AirVibe Starter Station uses a headless installation workflow.

The station does not require:

- a monitor
- a keyboard
- a mouse

After the first boot, the station should be accessible remotely through the network.

### Wireless LAN Configuration

When using Wi-Fi, configure:

- wireless network name
- wireless network password
- wireless LAN country

Verify that the wireless network details are correct before continuing.

Incorrect Wi-Fi settings may prevent the station from being accessible after the first boot.

### Locale Configuration

Configure the appropriate:

- timezone
- keyboard layout
- locale

These settings should match the installation location.

---

# Step 8 — Save the Advanced Options

Review the configured settings.

Save the advanced options and return to the main Raspberry Pi Imager window.

Verify that the operating system image and storage device remain correctly selected.

---

# Step 9 — Write the Operating System Image

<div align="center">
  <img src="images/08-write-sd-card.jpg" alt="Write SD Card" width="60%">
</div>

Select **Write** to begin writing the operating system image to the microSD card.

When the warning appears, confirm that the selected storage device may be erased.

### Important

All existing data on the selected microSD card will be permanently deleted.

Do not continue unless the correct storage device has been selected.

---

# Step 10 — Wait for Writing and Verification

<div align="center">
  <img src="images/09-writing-sd-card.jpg" alt="Writing SD Card" width="60%">
</div>

Raspberry Pi Imager performs two operations:

1. Writes the operating system image.
2. Verifies the written data.

Do not:

- remove the microSD card
- disconnect the card reader
- shut down the computer
- interrupt Raspberry Pi Imager

Allow both operations to complete successfully.

---

# Step 11 — Confirm Successful Completion

<div align="center">
  <img src="images/10-write-complete.jpg" alt="Write Complete" width="60%">
</div>

When Raspberry Pi Imager reports that writing has completed successfully, select **Continue**.

The microSD card now contains:

- the AirVibe reference operating system
- the configured hostname
- the configured user account
- SSH configuration
- network configuration
- locale settings

---

# Step 12 — Remove the microSD Card

Safely eject the microSD card from the operating system.

Remove the card from the card reader only after the computer confirms that it can be removed safely.

---

# Step 13 — Insert the Prepared microSD Card

<div align="center">
  <img src="images/11-insert-prepared-microsd-card.jpg" alt="Insert Prepared microSD Card" width="60%">
</div>

Insert the prepared microSD card into the Raspberry Pi.

Ensure that the card is fully seated before connecting power to the station.

Do not power on the Raspberry Pi until the microSD card has been inserted correctly.

---

# Final Verification

Before continuing, verify that:

- Raspberry Pi Imager completed without errors.
- Image verification completed successfully.
- The correct reference image was used.
- The correct microSD card was selected.
- SSH was enabled.
- The hostname was configured.
- User credentials were configured.
- Network settings were configured.
- Locale and timezone settings were configured.
- The microSD card was safely ejected.
- The microSD card was inserted correctly into the Raspberry Pi.

Completing these checks confirms that the microSD card is ready for the AirVibe Starter Station first boot procedure.

---

# Final Verification Checklist

- [ ] Raspberry Pi Imager opened successfully.
- [ ] Verified AirVibe reference image selected.
- [ ] Correct microSD card selected.
- [ ] Image and storage selections verified.
- [ ] Hostname configured.
- [ ] Username and password configured.
- [ ] SSH enabled.
- [ ] Wireless LAN configured if required.
- [ ] Wireless LAN country configured.
- [ ] Locale configured.
- [ ] Timezone configured.
- [ ] Keyboard layout configured.
- [ ] Operating system image written successfully.
- [ ] Written data verified successfully.
- [ ] microSD card safely ejected.
- [ ] microSD card inserted into the Raspberry Pi.
- [ ] System ready for the first boot procedure.

---

# Related Documents

- [REFERENCE DOC A — Reference Operating System](../00-Reference/reference-os.md)
- DOC 5 — First Boot (`first-boot.md`)

---

## Next Document

Continue with:

`DOC 5 — first-boot.md`

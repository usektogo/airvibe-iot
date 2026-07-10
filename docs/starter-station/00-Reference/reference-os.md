# REFERENCE DOC A — Reference Operating System

**Document ID:** REFERENCE DOC A  
**Version:** 1.0.0  
**Status:** Released  
**Last Updated:** 2026-07-03

---

# Overview

This document defines the reference operating system used by the AirVibe Starter Station reference implementation.

The purpose of this document is to ensure that every AirVibe Starter Station is built from the same verified software platform, providing a reproducible, stable, and well-tested deployment.

All Starter Station documentation assumes the operating system described in this document unless explicitly stated otherwise.

---

# Purpose

The goal of this document is to provide a reproducible operating system reference for AirVibe Starter Station deployments.

Rather than relying on the latest available operating system, the project uses a single verified reference image that has been tested on the reference hardware and validated throughout the complete documentation workflow.

---

# Reference Operating System

Operating system:

- Raspberry Pi OS Lite
- Legacy Bullseye
- 32-bit

Reference image:

`2023-05-03-raspios-bullseye-armhf-lite.img.xz`

---

# Supported Hardware

The reference operating system has been verified on:

- Raspberry Pi Zero W v1.1
- Raspberry Pi Zero WH v1.1
- Raspberry Pi Zero 2 WH

---

# Why AirVibe Uses Legacy (Bullseye)

AirVibe Starter Station documentation is based on Raspberry Pi OS Lite Legacy (Bullseye 32-bit).

This is an intentional engineering decision.

The goal of the Starter Station is to provide:

- reproducibility
- stability
- simplicity
- long-term maintainability

rather than using the newest Raspberry Pi OS release.

## Why Not Bookworm?

Bookworm introduced significant changes compared to Bullseye.

In particular:

- networking is handled differently
- NetworkManager is used by default
- several traditional Raspberry Pi configuration methods behave differently

For the AirVibe Starter Station reference implementation, these changes introduce unnecessary complexity without providing practical benefits.

Bullseye Legacy continues to support the proven workflow used across existing AirVibe Starter Station deployments.

---

## Benefits of the Reference Operating System

The selected operating system has been:

- verified on the `aq-off` reference station
- verified on multiple AirVibe deployments
- proven stable on Raspberry Pi Zero W v1
- validated for headless deployment
- validated for SSH-first installation
- validated for classic Wi-Fi configuration
- used throughout the complete AirVibe documentation project

This provides a consistent and reproducible installation process for every documented Starter Station.

---

## Important

The AirVibe Starter Station documentation is written, tested, and validated using the operating system described in this document.

Although newer Raspberry Pi OS releases may also work, they are outside the scope of the current reference implementation until they have been fully tested and documented.

---

# Engineering Principle

Use the platform that is known to work reliably.

Do not change operating systems simply because a newer version exists.

Reproducibility is more important than running the latest software release.

---

# Obtaining the Reference Image

Download the Raspberry Pi OS Lite Legacy Bullseye (32-bit) image from the official Raspberry Pi download archive.

Reference image:

`2023-05-03-raspios-bullseye-armhf-lite.img.xz`

This image is later selected in Raspberry Pi Imager using:

**Choose OS → Use custom**

---

# Image Verification

Reference image:

`2023-05-03-raspios-bullseye-armhf-lite.img.xz`

File size:

`363 MB (381,558,864 bytes)`

SHA256 checksum:

`B5E3A1D984A7EAA402A6E078D707B506B962F6804D331DCC0DAA61DEBAE3A19A`

Users are encouraged to verify downloaded files before use.

The checksum above applies to the verified reference image used throughout the AirVibe Starter Station documentation.

---

# GitHub Release Distribution

Future AirVibe releases may include the reference operating system image as part of a GitHub Release.

Example:

```text
AirVibe Starter Station v1
├── 2023-05-03-raspios-bullseye-armhf-lite.img.xz
└── SHA256SUMS.txt
```

---

# Summary

The AirVibe Starter Station reference implementation is based on:

- Raspberry Pi OS Lite
- Legacy Bullseye
- 32-bit

Reference image:

`2023-05-03-raspios-bullseye-armhf-lite.img.xz`

All AirVibe Starter Station documentation is based on this verified reference operating system and installation image to ensure a consistent and reproducible deployment.

---

## Next Document

Continue with:

`DOC 4 — Operating System Installation`

# DOC 2 — AirVibe Starter Station v1 Bill of Materials (BOM)

**Document ID:** DOC 2  
**Version:** 1.0.0  
**Status:** Released  
**Last Updated:** 2026-07-03

---

# Overview

This document lists the hardware required to build the AirVibe Starter Station v1 reference implementation.

It identifies the required and optional components, provides approximate pricing for planning purposes, and highlights important purchasing considerations.

Prices are approximate and intended only as a planning reference.

**Price estimate date:** June 2026

Prices may vary depending on supplier, region, VAT, shipping costs, and product availability.

---

# Required Components

| Component | Qty | Required | Approx. Price (€) | Notes |
|------------|-----|----------|-------------------|--------|
| Raspberry Pi Zero W v1 | 1 | Yes | 18–25 | Reference platform |
| Pimoroni Enviro+ | 1 | Yes | 50–60 | Environmental sensor board |
| PMS5003 | 1 | Yes | 20–30 | Particulate matter sensor |
| microSD Card (32 GB) | 1 | Yes | 8–12 | Class 10 recommended |
| Raspberry Pi Power Supply | 1 | Yes | 8–12 | Official Raspberry Pi power supply recommended |

---

# Supported Alternative

| Component | Approx. Price (€) | Notes |
|------------|-------------------|--------|
| Raspberry Pi Zero 2 W | 20–30 | Fully supported alternative to Raspberry Pi Zero W v1 |

The Starter Station documentation is based on a Raspberry Pi Zero W v1 reference implementation.

The Raspberry Pi Zero 2 W is also supported and may provide improved performance and better availability.

---

# Optional Components

| Component | Approx. Price (€) | Purpose |
|------------|-------------------|---------|
| TFA Radiation Shield | 15–25 | Improves outdoor temperature measurements |

---

# Estimated Project Cost

| Configuration | Estimated Cost (€) |
|---------------|-------------------:|
| Starter Station | 110–125 |
| Starter Station + TFA Radiation Shield | 125–150 |

These estimates exclude:

- Shipping costs
- Import duties
- Local taxes
- Optional accessories

---

# Reference Suppliers

The following suppliers are commonly used when sourcing components for the AirVibe Starter Station reference implementation:

- Pimoroni
- The Pi Hut

Equivalent components may also be purchased from other reputable suppliers.

---

# Purchasing Notes

Before ordering components:

- Verify component availability.
- Confirm Raspberry Pi model compatibility.
- Check whether the PMS5003 sensor includes the required connection cable.
- Verify that the power supply plug type matches your region.
- Consider ordering a spare microSD card for backup or future maintenance.

---

## Next Document

Continue with:

`02-Assembly/assembly.md`

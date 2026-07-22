# AirVibe Starter Station Documentation

Follow the documents in order to build and prepare the verified AirVibe Starter Station reference implementation.

> **Status:** Documentation preview  
>
> The documents listed below have been verified and released within the documentation branch. The official project release will follow after the complete documentation set and final release review are finished.

---

## Start Here

| Step | Document | Purpose |
|------|----------|---------|
| Reference | [Reference Operating System](00-Reference/reference-os.md) | Defines the verified operating system baseline, reference image, supported hardware, and image checksum. |
| 1 | [Hardware](01-Hardware/hardware.md) | Introduces the required hardware components and supported measurements. |
| 2 | [Bill of Materials](01-Hardware/bill-of-materials.md) | Lists the required and optional components, estimated costs, and purchasing considerations. |
| 3 | [Assembly Guide](02-Assembly/assembly.md) | Explains how to assemble the Starter Station hardware. |
| 4 | [Operating System Installation](03-Software/flash-sd-card.md) | Prepares the microSD card, configures the reference operating system, and installs the prepared card in the Raspberry Pi. |
| 5 | [First Boot](03-Software/first-boot.md) | Verifies startup, Wi-Fi connectivity, network discovery, SSH access, and operating system updates. |
| 6 | [Enable Interfaces](03-Software/enable-interfaces.md) | Enables and verifies the I2C and hardware serial interfaces required by the reference implementation. |

Complete the documents in the listed order. Each procedure assumes that the preceding document has been completed successfully.

---

## Current Documentation Coverage

The current documentation set covers:

- reference operating system selection and verification
- hardware selection
- bill of materials
- physical assembly
- microSD card preparation
- headless first boot
- Wi-Fi and SSH verification
- operating system updates
- I2C and hardware serial interface configuration

---

## Next Document

The next planned document is:

- [DOC 7 — Install Enviro+](03-Software/install-enviro-plus.md)

DOC 7 is currently being verified and will be added to the released documentation set after review and approval.

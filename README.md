# DoE (Data Over Ethernet) - Microcontroller Communication Protocol

DoE is an experimental communication protocol designed for microcontrollers, utilizing Ethernet cabling (RJ45) and RS485 differential signaling to establish a custom network stack. The project focuses on bridging low-level hardware (AVR/Raspberry Pi Pico) with a structured, IP-like frame logic.

---
```
### 1. Frame Structure
The protocol uses a fixed-size header with a large payload area, totaling 128 bytes per frame.

- Entry SYNC (4B): Synchronization sequence (0xDEFDEFDE or 0x5533CCD5).
- Control (4B):
    - Category: Protocol standard version.
    - Flags: Communication status (ACK, NULL, DENY, etc.).
    - Network Settings: Operations like Network Reset (RST) or DHCP renew.
    - TTL: Time To Live (default 64, decremented per hop).
- Source IP (2B): Originating host address.
- Gateway (2B): Network gateway/router address.
- Target IP (2B): Destination host address.
- Payload (112B): Data content.
- Checksum (2B): CRC16 for error detection.
```
---
```
### 2. Network Logic and Routing

Automatic Configuration (DHCP):
The router performs automatic configuration by sending a category 1 frame with DHCP flags. This allows nodes to receive an IP assignment from the available pool within the payload.

External Communication:
To communicate outside the local segment, a host sets the Target IP to the router's address. The frame is then processed by the gateway for further routing.

Acknowledgment System:
The protocol supports delivery confirmation using ACK flags within the Control byte, ensuring reliable data transfer between nodes.
```
---
```
### 3. Hardware Implementation (PICO-CISCO)

The infrastructure, referred to as PICO-CISCO, acts as the central routing unit.

Core Processor: Raspberry Pi Pico 2W.

Node Architectures:
- Standard Node: AVR microcontrollers connected via UART to RS485 transceivers.
- Advanced Node: Raspberry Pi Pico utilizing dual-core processing for hardware-level UART/I2C management and dual RJ45 ports.

Internal Communication:
Communication between the main processor and nodes is handled via I2C with an additional I2C_IRQ signal. This interrupt-driven approach allows the main processor to efficiently detect and process incoming data from specific nodes.
```
---
```
### 4. Physical Layer Specifications
- Physical Medium: RJ45 (Ethernet cable).
- Signaling: RS485 Differential signal (Transitioned from manual GPIO bit-banging/PIO).
- Target Speeds: 1, 2, 4, 8, 16, 32, 64 kb/s.
- Range: 0.25 to 25 meters.
- Voltage: 3.3V (Logic) with 5V RS485 transceivers (requires logic level conversion).
```
---
```
### 5. Advanced Node Features (RPI PICO)
Utilizing the Raspberry Pi Pico as a node allows for offloading responsibilities from the main processor. Features include:
- Dual-core handling for hardware UART and I2C.
- Priority Management: Implementation of "Fast" or "Priority" flags in the Control section for urgent packets.
- High RAM Capacity: Enables complex buffering and local protocol handling that exceeds basic "translator" capabilities.
```
---
```
### 6. Disclaimer
This documentation represents an ongoing development phase (Data Over Ethernet) and is subject to revision regarding the final physical layer implementation and frame synchronization standards.
```
---
Created by Demichu5

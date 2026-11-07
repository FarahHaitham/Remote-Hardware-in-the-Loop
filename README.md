# Remote Hardware-in-the-Loop (HIL) System for FPGA Configuration

This project implements a remote Hardware-in-the-Loop (HIL) testing environment that enables seamless configuration and control of a Basys3 FPGA. By utilizing a Raspberry Pi as an intermediary controller, the system bridges a host PC to the FPGA, allowing users to remotely upload synthesized bitstreams and interact with the hardware in real-time without physical access to the board.

## System Architecture & Key Features

* **Heterogeneous Networking:** Leverages TCP sockets for the reliable, error-free bulk transfer of configuration files, paired with MQTT (HiveMQ) for high-frequency, low-latency control telemetry.

* **Cross-Network VPN Tunneling:** Integrates Tailscale VPN to assign static IPs across different networks, utilizing custom 1024-byte packet chunking to bypass MTU constraints and prevent fragmentation.

* **Lightweight JTAG Programming:** Uses the open-source `openFPGALoader` library to flash the FPGA directly from the Raspberry Pi over USB, eliminating the need for bulky proprietary toolchains like Xilinx Vivado.

* **High-Speed Hardware I/O:** Once the board is programmed, communication shifts to a 1 MHz Serial Peripheral Interface (SPI) for deterministic, high-speed data exchange between the Pi and the FPGA.

* **Multi-Threaded Concurrency:** The Raspberry Pi controller runs on a highly concurrent architecture with isolated, mutually exclusive threads (secured by Mutex locks) for TCP listening, MQTT callbacks, and continuous SPI hardware looping to prevent I/O bottlenecks.

* **Use Case:** The current graphical user interface (GUI) and data parsing logic are specialized for testing an Arithmetic Logic Shift Unit (ALSU).
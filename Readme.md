![Sharif University - IoT Project](Miscellaneous/logo.png)

# ESP32 OTA Firmware Update Management System

## Table of Contents
1. [About The Project](#about-the-project)
2. [Tools](#tools)
3. [Getting Started](#getting-started)
   - [Implementation Details](#implementation-details)
   - [How to Run](#how-to-run)
4. [Results](#results)
5. [Related Links](#related-links)
6. [Authors](#authors)

## About The Project

This project implements an **Over-the-Air (OTA) firmware update system for ESP32 devices**.

The system allows an ESP32 to receive firmware updates remotely through a FastAPI server. It supports manual and automatic updates, SHA-256 integrity verification, OTA rollback, periodic heartbeat, update status tracking, and firmware management through a web panel.

The project is organized into three main parts:

- **ESP32 OTA Client**: handles Wi-Fi connection, update checks, download, verification, installation, validation, and rollback.
- **Backend Server**: manages devices, firmware files, update jobs, and update history.
- **Web Panel**: provides a simple interface for managing devices, firmware versions, and updates.

## Tools

The main tools and technologies used in this project are:

- **ESP32 DevKitC V4 / ESP-WROOM-32**
- **ESP-IDF 6.0.2**
- **C / FreeRTOS**
- **Python / FastAPI**
- **SQLite**
- **HTML / CSS / JavaScript**
- **HTTP / JSON**
- **mbedTLS (SHA-256)**
- **NVS and ESP-IDF OTA APIs**

## Getting Started

The main project files are organized as follows:

```text
Code/
├── esp32_ota_client/
├── server/
└── web_panel/

Document/
Miscellaneous/
```

The `Miscellaneous` folder contains the project proposal and weekly progress reports.

### Implementation Details

#### ESP32 OTA Client

The ESP32 client is located in:

```text
Code/esp32_ota_client/
```

Its main responsibilities are:

- Connecting to Wi-Fi
- Registering the device on the server
- Sending heartbeat messages
- Checking for firmware updates
- Downloading firmware through HTTP
- Verifying firmware with SHA-256
- Installing firmware on OTA partitions
- Saving OTA state in NVS
- Validating new firmware after reboot
- Rolling back to the previous firmware if validation fails

The firmware uses `factory`, `ota_0`, and `ota_1` application partitions.

#### Backend Server

The backend is located in:

```text
Code/server/
```

It is implemented with FastAPI and SQLite and provides APIs for:

- Device registration
- Manual and automatic updates
- Firmware upload and download
- Firmware activation/deactivation
- Update status reporting
- Update history

The server also prevents duplicate open update jobs and avoids automatically retrying firmware versions that previously failed or rolled back on the same device.

#### Web Panel

The management panel is located in:

```text
Code/web_panel/
```

It provides pages for:

- Devices
- Firmware versions
- Update jobs
- Update history

## How to Run

### 1. Run the Server

```bash
cd Code/server
./scripts/01_setup.sh
./scripts/02_run.sh
```

The server runs on port `8000` by default.

Useful URLs:

```text
http://localhost:8000/panel/
http://localhost:8000/docs
http://localhost:8000/api/health
```

### 2. Configure the ESP32

Update the Wi-Fi configuration in:

```text
Code/esp32_ota_client/main/wifi_config.h
```

Update the server address in:

```text
Code/esp32_ota_client/main/api_config.h
```

Also check the ESP-IDF path and serial port in:

```text
Code/esp32_ota_client/scripts/01_env.sh
```

### 3. Build and Flash the ESP32

```bash
cd Code/esp32_ota_client
./scripts/02_deploy.sh
```

To monitor serial output:

```bash
./scripts/03_run.sh
```

To clean build files:

```bash
./scripts/04_clean.sh
```

## Results

The final system was tested successfully for the main OTA scenarios, including:

- Normal OTA update
- SHA-256 verification and mismatch rejection
- Switching between `ota_0` and `ota_1`
- Firmware validation and rollback
- Manual update mode
- Automatic update mode
- Active/inactive firmware management
- Periodic heartbeat and update checks
- Wi-Fi disconnection and reconnection
- Server recovery
- Prevention of repeated failed updates

The project also resolved reliability issues found during development, including repeated rollback loops and stack overflow in the periodic OTA service.

## Related Links

- [ESP-IDF Documentation](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/)
- [ESP-IDF OTA API](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/system/ota.html)
- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [SQLite Documentation](https://www.sqlite.org/docs.html)
- [Project Repository](https://github.com/Sharif-University-ESRLab/summer2026-firmware-update-OTA)

## Authors

- **Ali Fathi**
- **Armin Farsi**
- **Amirreza Bahrami**

Internet of Things Course  
Sharif University of Technology  
Summer 1405 / 2026

# Automated Warehouse Control and Monitoring System

**Academic Project | Ho Chi Minh City University of Technology (HCMUT), VNU-HCM | 2026**

An integrated **automated storage and retrieval system (AS/RS) prototype** combining a simulated warehouse, PLC-based control, MQTT messaging, a Node-RED operator dashboard, and the open-source GreaterWMS platform. This is an individual academic engineering project.

> **Scope:** This is a simulation-based prototype, not a deployed industrial warehouse. The project demonstrates control, monitoring, and selected inventory synchronization workflows.

## 1. Project Overview

The system models a warehouse with **54 storage locations**. Operators can select a bin and issue **STORE** or **RETRIEVE** commands through a Node-RED dashboard. PLC logic coordinates the simulated material-handling equipment in Factory I/O. MQTT carries commands and bin-status messages, while Node-RED uses GreaterWMS REST APIs to display and update selected inventory information.

### Main capabilities

- Simulated storage and retrieval operations for a 54-bin warehouse.
- PLC control logic developed in Siemens TIA Portal.
- Node-RED controls for **START**, **STOP**, **STORE**, **RETRIEVE**, and **RESET WAREHOUSE**.
- MQTT-based command transmission and bin-status monitoring.
- Dashboard views for total, empty, and occupied bins, including a summary chart.
- GreaterWMS REST API integration for stock and bin information.
- Inventory tables showing stock codes, bin assignments, and empty/occupied status.

## 2. System Architecture

![System architecture diagram](docs/system-architecture.png)

*Figure 1. System architecture of the automated warehouse prototype (diagram provided by the project author).*

The diagram illustrates the system's principal data path: **Factory I/O** simulates warehouse equipment and communicates with the **Siemens S7-1500 PLC**; the PLC publishes data through an **MQTT broker**, which **Node-RED** subscribes to and processes. From Node-RED, **REST APIs** connect to **GreaterWMS** for inventory data, while the **web UI** presents dashboard monitoring. The diagram emphasizes the monitoring/data flow; operator commands and return paths are also implemented in the Node-RED flows and should not be inferred solely from the one-way arrows shown.

The repository does not include a hosted or ready-to-run GreaterWMS instance.

## 3. Technology Stack

| Layer | Technology | Role |
|---|---|---|
| Simulation | Factory I/O | Warehouse scene and material-handling simulation |
| Control | Siemens TIA Portal V17 / S7-1500 project | PLC logic and sequencing |
| Messaging | MQTT | Control commands and bin-status messages |
| Integration and HMI | Node-RED | Dashboard, message processing, API integration |
| Warehouse management | GreaterWMS (open source) | Stock and bin information |
| Interfaces | HTTP REST / JSON | GreaterWMS data exchange |

## 4. Node-RED Implementation

The exported Node-RED project contains three main flow tabs:

| Flow tab | Function |
|---|---|
| `General` | Fetches warehouse-bin information and displays total, empty, and occupied counts with a chart |
| `Display detail` | Retrieves paginated stock/bin records, combines results, removes duplicates where applicable, and renders tables |
| `PLC S7` | Provides operator controls, sends MQTT commands, receives bin status, and builds GreaterWMS update requests |

**Examples of MQTT topics in the flow:**

```text
factoryio/warehouse/cmd/store
factoryio/warehouse/cmd/retrieve
factoryio/warehouse/cmd/system
factoryio/warehouse/bin_status
```

**Examples of GreaterWMS endpoints used:**

```text
GET /stock/list/
GET /stock/bin/
GET /binset/?empty_label=true
GET /binset/?empty_label=false
```

The control flow also contains prototype-specific bin-update and stock/bin-update requests. These depend on the local GreaterWMS configuration and are not presented as a general-purpose WMS connector.

## 5. My Implementation

This is an **individual project**. My implementation and integration work includes:

- Configuring and integrating the Factory I/O warehouse simulation and Siemens PLC project.
- Developing the Node-RED dashboard for warehouse status and inventory information.
- Implementing Node-RED logic to retrieve and process GreaterWMS REST API responses.
- Handling paginated API responses and preparing records for dashboard tables.
- Implementing dashboard commands and MQTT message handling for warehouse operations.
- Integrating received bin-status messages with prototype GreaterWMS update requests.
- Testing the integrated simulated warehouse workflow.

GreaterWMS is an existing open-source platform used in the project; I do **not** claim to have developed the GreaterWMS software itself.

## 6. Demonstration

A representative test sequence uses **Bin05**:

1. Select the target bin from the Node-RED dashboard.
2. Send a **STORE** command and observe the simulated warehouse operation.
3. Check the resulting bin-status update on the dashboard and WMS interface.
4. Send a **RETRIEVE** command for the same bin.
5. Verify that the bin returns to the empty state.

Screenshots and/or a short demonstration video can be added to this section:

<!-- Replace the placeholders below with actual files or links after adding them. -->
- Factory I/O warehouse overview: *to be added*
- Node-RED dashboard: *to be added*
- GreaterWMS inventory view: *to be added*
- STORE / RETRIEVE demonstration video: *to be added*

## 7. Repository Structure

```text
automated_warehouse_portfolio/
├── README.md
├── docs/           # System architecture diagram
│   └── system-architecture.png
├── factory-io/     # Factory I/O scene file
├── node-red/       # Sanitized Node-RED flow export (JSON)
└── plc/            # Archived TIA Portal V17 project (.zap17)
```

## 8. How to Inspect the Project

- **PLC:** Open the archive using the compatible Siemens TIA Portal version (V17). PLC hardware configuration and dependencies may be required to run the project.
- **Factory I/O:** Open the scene using a compatible Factory I/O installation.
- **Node-RED:** Import the JSON file via **Menu → Import**. Install the additional Node-RED nodes used by the project as required, then configure the MQTT broker, PLC connection, and GreaterWMS API settings for your own environment.
- **GreaterWMS:** Install and configure GreaterWMS separately, following the documentation and license of the original open-source project. Its source code is **not redistributed here**.

**Security:** The shared Node-RED export is intended to be sanitized. Replace placeholder endpoints and credentials locally. Do not commit active API tokens, passwords, private keys, or private database dumps. Review the repository contents before publishing it.

## 9. Limitations and Future Work

- The warehouse is simulated; no physical deployment or industrial reliability validation is claimed.
- Some bin-to-database-ID mappings are configured for the demonstration dataset.
- Some inventory-update logic uses a fixed demonstration goods code rather than a general product-tracking workflow.
- API pagination and synchronization logic reflect the test environment and may require adaptation for larger datasets or other GreaterWMS versions.
- Potential improvements include configurable bin mapping, richer product and quantity tracking, better error handling, and more extensive end-to-end testing.

## 10. Acknowledgments

Developed as an individual academic project at **Ho Chi Minh City University of Technology (HCMUT), VNU-HCM**. The project uses third-party engineering tools and the open-source GreaterWMS platform; their respective rights and licenses remain with their owners.

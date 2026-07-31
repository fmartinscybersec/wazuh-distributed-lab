# Wazuh Distributed Architecture Lab

---

## Project Status

```text
Status: Work in Progress (WIP)

The core installation guides are already available.

The remaining documentation, Proof of Concepts (PoCs), and additional examples are currently being written and will be published progressively.
```
---

A comprehensive, production-ready lab that demonstrates how to deploy a **distributed Wazuh SIEM/XDR environment** by separating the **Indexer**, **Server**, and **Dashboard** into dedicated virtual machines.

Unlike most tutorials that rely on an **All-in-One** deployment, this project follows a **multi-node architecture**, providing a realistic environment for learning how Wazuh is deployed in enterprise Security Operations Centers (SOCs).

The repository includes step-by-step installation guides, configuration examples, and practical Proof of Concept (POC) scenarios to validate detection and response capabilities.

---

# Project Goals

This project aims to help students, cybersecurity professionals, and SOC analysts understand how to deploy and manage a distributed Wazuh environment.

The objectives include:

- Deploy a production-style distributed Wazuh architecture.
- Understand the role of each Wazuh component.
- Configure secure communication using TLS certificates.
- Document every installation step with screenshots.
- Demonstrate detection capabilities through practical POC scenarios.

---

# Architecture Overview

The laboratory is composed of three independent components:

| Component | Description |
|-----------|-------------|
| **Wazuh Indexer** | Stores, indexes, and searches security events. |
| **Wazuh Server** | Collects, analyzes, correlates, and manages security events received from monitored endpoints. |
| **Wazuh Dashboard** | Provides visualization, investigation, threat hunting, and platform administration. |

---

# Documentation

Follow the documentation in the order below.

| Step | Documentation |
|------|---------------|
| 01 | Wazuh Indexer |
| 02 | Wazuh Server |
| 03 | Wazuh Dashboard |
| 04 | Password Management |
| 05 | Create Admin Users |
| 06 | Apply Patch 4.14.6 *(Optional)* |
| 07 | Agent Authentication |
| 08 | Remove Agent |
| 09 | Generate API Token |


> **Note:** Step **06 – Apply Patch 4.14.6** is **optional** and only applies to existing Wazuh deployments that require an upgrade to version **4.14.6**. It is **not required** for a clean installation.

---

# Proof of Concepts

After completing the installation, the following practical scenarios can be used to validate the deployment.

| POC | Description |
|-----|-------------|
| 01 | Blocking a Known Malicious Actor |
| 02 | File Integrity Monitoring |
| 03 | Detecting a Brute-Force Attack |
| 04 | Detecting Unauthorized Processes |

---

# Repository Structure

```text
wazuh-distributed-lab/
│
├── README.md
├── LICENSE
├── .gitignore
│
├── docs/
│   ├── 01-wazuh-indexer.md
│   ├── 02-wazuh-server.md
│   ├── 03-wazuh-dashboard.md
│   ├── 04-password-management.md
│   ├── 05-admin-users.md
│   ├── 06-apply-patch-4.14.6.md
│   ├── 07-agent-authentication.md
│   ├── 08-remove-agent.md
│   └── 09-api-token.md
│
├── poc/
│   ├── 01-block-known-malicious-actor.md
│   ├── 02-file-integrity-monitoring.md
│   ├── 03-brute-force-detection.md
│   └── 04-unauthorized-processes.md
│
└── assets/
    └── screenshots/
        ├── 01-wazuh-indexer/
        ├── 02-wazuh-server/
        ├── 03-wazuh-dashboard/
        ├── 04-password-management/
        ├── 05-admin-users/
        ├── 06-apply-patch-4.14.6/
        ├── 07-agent-authentication/
        ├── 08-remove-agent/
        ├── 09-api-token/
        ├── poc-01-block-known-malicious-actor/
        ├── poc-02-file-integrity-monitoring/
        ├── poc-03-brute-force-detection/
        └── poc-04-unauthorized-processes/
```

---

# Lab Environment

This project was developed using:

- Ubuntu Server
- Wazuh 4.14.6
- Distributed Architecture
- Virtual Machines
- TLS Certificate Authentication
- Filebeat
- UFW Firewall

---

# Screenshots

Each document contains its own screenshots.

All screenshots are stored under:

```text
assets/screenshots/
```

Each documentation chapter and Proof of Concept has its own dedicated folder to simplify maintenance and future updates.

---

# License

This project is distributed under the **MIT License**.

See the **LICENSE** file for more information.
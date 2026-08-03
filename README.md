# Wazuh Distributed Architecture Lab

## Project Status

```text
Status: Work in Progress (WIP)

The core installation guides are already available.

The remaining documentation, Proof of Concepts (PoCs), and additional examples are currently being written and will be published progressively.
```

---

A comprehensive, production-ready lab that demonstrates how to deploy a **distributed Wazuh SIEM/XDR environment** by separating the **Indexer**, **Server**, and **Dashboard** into dedicated virtual machines.

Unlike most tutorials that rely on an **All-in-One** deployment, this project follows a **multi-node architecture**, providing a realistic environment for learning how Wazuh is deployed in enterprise Security Operations Centers (SOCs).

The repository includes step-by-step installation guides, configuration examples, maintenance procedures, and practical Proof of Concept (POC) scenarios to validate detection and response capabilities.

---

## Project Goals

This project aims to help students, cybersecurity professionals, and SOC analysts understand how to deploy and manage a distributed Wazuh environment.

The objectives include:

- Deploy a production-style distributed Wazuh architecture.
- Understand the role of each Wazuh component.
- Configure secure communication using TLS certificates.
- Document every installation step with screenshots.
- Demonstrate detection capabilities through practical POC scenarios.

---

## Architecture Overview

The laboratory is composed of three independent components:

| Component | Description |
|-----------|-------------|
| **Wazuh Indexer** | Stores, indexes, and searches security events. |
| **Wazuh Server** | Collects, analyzes, correlates, and manages security events received from monitored endpoints. |
| **Wazuh Dashboard** | Provides visualization, investigation, threat hunting, and platform administration. |

---

## Documentation

Follow the documentation in the order below.

| Step | Guide | Status |
|------|-------|:------:|
| 01 | [Wazuh Indexer](docs/01-wazuh-indexer.md) | Completed |
| 02 | [Wazuh Server](docs/02-wazuh-server.md) | Completed |
| 03 | [Wazuh Dashboard](docs/03-wazuh-dashboard.md) | Completed |
| 04 | [Password Management](docs/04-password-management.md) | Completed |
| 05 | [Administrator Users](docs/05-admin-users.md) | Completed |
| 06 | [Apply Patch](docs/06-apply-patch.md) | Completed |
| 07 | [Agent Authentication](docs/07-agent-authentication.md) | Completed |
| 08 | [Remove Agent](docs/08-remove-agent.md) | Completed |
| 09 | [Generate API Token](docs/09-api-token.md) | Completed |


> **Note:** Step **06 – Apply Patch** is **optional** and only applies to existing Wazuh deployments that require an upgrade to **new version**. It is **not required** for a clean installation.

---

## Proof of Concepts

After completing the installation, the following practical scenarios can be used to validate the deployment.

| PoC | Guide | Status |
|-----|-------|:------:|
| 01 | [Blocking a Known Malicious Actor](poc/01-block-known-malicious-actor.md) | WIP |
| 02 | [File Integrity Monitoring](poc/02-file-integrity-monitoring.md) | WIP |
| 03 | [Detecting a Brute-Force Attack](poc/03-brute-force-detection.md) | WIP |
| 04 | [Detecting Unauthorized Processes](poc/04-unauthorized-processes.md) | WIP |
| 05 | [Network IDS Integration](poc/05-network-ids-integration.md) | WIP |
| 06 | [Detecting an SQL Injection Attack](poc/06-detecting-an-sql-injection-attack.md) | WIP |
| 07 | [Detecting Suspicious Binaries](poc/07-detecting-suspicious-binaries.md) | WIP |
| 08 | [Detecting and Removing Malware Using VirusTotal](poc/08-detecting-and-removing-malware-using-virustotal-integration.md) | WIP |
---

## Repository Structure

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
│   ├── 04-unauthorized-processes.md
│   ├── 05-network-ids-integration.md
│   ├── 06-detecting-an-sql-injection-attack.md
│   ├── 07-detecting-suspicious-binaries.md
│   └── 08-detecting-and-removing-malware-using-virustotal-integration.md
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
        ├── poc-04-unauthorized-processes/
        ├── poc-05-network-ids-integration/
        ├── poc-06-detecting-an-sql-injection-attack/
        ├── poc-07-detecting-suspicious-binaries/
        └── poc-08-detecting-and-removing-malware-using-virustotal/
```

---

## Lab Environment

This project was developed using:

- Ubuntu Server
- Wazuh 4.14.6
- Distributed Architecture
- Virtual Machines
- TLS Certificate Authentication
- Filebeat
- UFW Firewall

---

## Screenshots

Each document contains its own screenshots.

All screenshots are stored under:

```text
assets/screenshots/
```

Each documentation chapter and Proof of Concept has its own dedicated folder to simplify maintenance and future updates.

---

## License

This project is distributed under the **MIT License**.

See the **LICENSE** file for more information.
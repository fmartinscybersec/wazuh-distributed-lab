# Wazuh Multi-Node Distributed Architecture Lab

A comprehensive, production-ready guide to deploying a distributed (multi-node) Wazuh SIEM/XDR cluster. This project rejects the "All-in-One" single-host shortcut, guiding security analysts through the architectural separation of components to achieve true scalability and isolation.

## 🧱 Architecture Overview

In this lab, the Wazuh architecture is broken down into three core layers:
1. **Wazuh Indexer:** A highly scalable, full-text search and analysis engine used to store and index security alerts.
2. **Wazuh Server:** The management node that receives data from agents, processes logs via the analysis engine, and triggers alerts.
3. **Wazuh Dashboard:** The web-based user interface for data visualization, threat hunting, and platform administration.

## 📁 Repository Structure

```text
wazuh-distributed-lab/
├── docs/
│   ├── 01-wazuh-indexer.md
│   ├── 02-wazuh-server.md
│   └── 03-wazuh-dashboard.md
└── assets/
    └── screenshots/
            ├── 📂 wazuh-indexer/
            ├── 📂 wazuh-server/
            └── 📂 wazuh-dashboard/

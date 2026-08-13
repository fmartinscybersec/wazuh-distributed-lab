# Network IDS Integration

---

# Table of Contents

1. Requirements
2. Update Packages
3. Install Suricata
4. Download and Extract the Suricata Rules
5. Modify the Suricata Configuration
6. Restart Suricata
7. Configure the Wazuh Agent
8. Modify the Wazuh Agent Configuration
9. Restart the Wazuh Agent
10. Test the Integration
11. Verify the Alerts in the Dashboard

---

## 1 - Requirements

The following resource sizing guidance was used during the preparation of this laboratory.

| Traffic Profile | CPU | Memory | Storage |
|---|---:|---:|---:|
| Domestic / Lab (up to 100 Mbps) | 2 cores | 4 GB | 20 GB to 50 GB |
| Small Business (up to 500 Mbps) | 4 to 8 cores | 8 GB to 16 GB | 100 GB to 500 GB (SSD recommended) |
| Medium Business (up to 1 Gbps) | 8 to 16 cores | 16 GB to 32 GB | 1 TB+ (high-performance SSD) |
| Large Scale (above 1 Gbps) | 16+ cores (Xeon/EPYC) | 64 GB to 128 GB+ | Multiple TBs in RAID / NVMe |

---

## 2 - Update Packages

Update the package list.

```bash
sudo apt update
```

Upgrade the installed packages.

```bash
sudo apt upgrade -y
```

---

## 3 - Install Suricata

Install Suricata on the dedicated Ubuntu endpoint.

Add the official OISF Suricata stable repository.

```bash
sudo add-apt-repository ppa:oisf/suricata-stable
```

![Add the Suricata stable repository](../assets/screenshots/poc-05-network-ids-integration/Fig_1.jpg)

Update the package list.

```bash
sudo apt-get update
```

Install Suricata.

```bash
sudo apt-get install suricata -y
```

![Install Suricata](../assets/screenshots/poc-05-network-ids-integration/Fig_2.jpg)

---

## 4 - Download and Extract the Suricata Rules

Download the Emerging Threats Suricata rules.

```bash
cd /tmp/ && curl -LO https://rules.emergingthreats.net/open/suricata-6.0.8/emerging.rules.tar.gz
```

![Download the Emerging Threats Suricata rules](../assets/screenshots/poc-05-network-ids-integration/Fig_3.jpg)

Extract the downloaded rules archive, create the Suricata rules directory, and move the rules into it.

```bash
sudo tar -xvzf emerging.rules.tar.gz && sudo mkdir /etc/suricata/rules && sudo mv rules/*.rules /etc/suricata/rules/
```

![Extract and move the Suricata rules](../assets/screenshots/poc-05-network-ids-integration/Fig_4.jpg)

Set the permissions for the Suricata rule files.

```bash
sudo find /etc/suricata/rules -name "*.rules" -exec chmod 777 {} \;
```

![Set permissions for the Suricata rules](../assets/screenshots/poc-05-network-ids-integration/Fig_5.jpg)

---

## 5 - Modify the Suricata Configuration

Open the Suricata configuration file.

```bash
sudo nano /etc/suricata/suricata.yaml
```

The network variables must be configured according to the Ubuntu endpoint environment.
![Open the Suricata configuration file](../assets/screenshots/poc-05-network-ids-integration/Fig_6.jpg)

To identify the Ubuntu IP address and network interface, run:

```bash
ip a
```

![Identify the Ubuntu IP address and network interface](../assets/screenshots/poc-05-network-ids-integration/Fig_7.jpg)

### Configure the Network Variables

Before making the changes:

![Suricata network variables before configuration](../assets/screenshots/poc-05-network-ids-integration/Fig_8.jpg)

After making the changes:

![Suricata network variables after configuration](../assets/screenshots/poc-05-network-ids-integration/Fig_9.jpg)

---

### Configure the Default Rule Set

Press:

```text
Ctrl + F
```

Search for:

```text
default-rule
```

Press Enter.

Before making the changes:

![Default rule configuration before editing](../assets/screenshots/poc-05-network-ids-integration/Fig_10.jpg)

After making the changes:

![Default rule configuration after editing](../assets/screenshots/poc-05-network-ids-integration/Fig_11.jpg)

---

### Enable Suricata Statistics

Press:

```text
Ctrl + F
```

Search for:

```text
global
```

Press Enter.

Confirm that the following configuration is enabled:

```yaml
stats:
  enabled: yes
```

![Suricata statistics configuration](../assets/screenshots/poc-05-network-ids-integration/Fig_12.jpg)

---

### Configure the Linux High Performance Interface

Press:

```text
Ctrl + F
```

Search for:

```text
linux high
```

Press Enter.

Before making the change:

![Linux high performance configuration before editing](../assets/screenshots/poc-05-network-ids-integration/Fig_13.jpg)

After making the change:

The Ubuntu network interface identified previously is `ens33`.

![Linux high performance configuration after editing](../assets/screenshots/poc-05-network-ids-integration/Fig_14.jpg)

Save the configuration file.

```text
Ctrl + X
Y
Enter
```

---

## 6 - Restart Suricata

Restart the Suricata service.

```bash
sudo systemctl restart suricata
```

![Restart the Suricata service](../assets/screenshots/poc-05-network-ids-integration/Fig_15.jpg)

---

## 7 - Configure the Wazuh Agent

Configure the Wazuh Agent on the Ubuntu endpoint where Suricata is installed.

The following screenshots document the Wazuh Agent configuration performed on the Ubuntu endpoint.

![Configure the Wazuh Agent](../assets/screenshots/poc-05-network-ids-integration/Fig_16.jpg)

![Configure the Wazuh Agent](../assets/screenshots/poc-05-network-ids-integration/Fig_17.jpg)

![Configure the Wazuh Agent](../assets/screenshots/poc-05-network-ids-integration/Fig_18.jpg)

![Configure the Wazuh Agent](../assets/screenshots/poc-05-network-ids-integration/Fig_19.jpg)

![Configure the Wazuh Agent](../assets/screenshots/poc-05-network-ids-integration/Fig_20.jpg)

![Configure the Wazuh Agent](../assets/screenshots/poc-05-network-ids-integration/Fig_21.jpg)

Verify in the Wazuh Dashboard that the Ubuntu agent is configured.

![Verify the Wazuh Agent in the Dashboard](../assets/screenshots/poc-05-network-ids-integration/Fig_22.jpg)

---

## 8 - Modify the Wazuh Agent Configuration

Open the Wazuh Agent configuration file.

```bash
sudo nano /var/ossec/etc/ossec.conf
```

![Open the Wazuh Agent configuration file](../assets/screenshots/poc-05-network-ids-integration/Fig_23.jpg)

Go to the end of the configuration file and add the following configuration.

This configuration tells the Wazuh Agent to read the Suricata events stored in the `eve.json` file.

```xml
<localfile>
  <log_format>json</log_format>
  <location>/var/log/suricata/eve.json</location>
</localfile>
```

![Configure the Suricata eve.json log source](../assets/screenshots/poc-05-network-ids-integration/Fig_24.jpg)

Save the file.

```text
Ctrl + X
Y
Enter
```

---

## 9 - Restart the Wazuh Agent

Restart the Wazuh Agent service.

```bash
sudo systemctl restart wazuh-agent
```

![Restart the Wazuh Agent](../assets/screenshots/poc-05-network-ids-integration/Fig_25.jpg)

---

## 10 - Test the Integration

From the Wazuh Server, send ping requests to the IP address of the Ubuntu endpoint running Suricata.

```bash
ping -c 20 192.168.232.168
```

![Test the network connection](../assets/screenshots/poc-05-network-ids-integration/Fig_26.jpg)

---

## 11 - Verify the Alerts in the Dashboard

Open the Wazuh Dashboard and verify the alerts generated by the Suricata integration.

![Verify the Suricata alerts in the Wazuh Dashboard](../assets/screenshots/poc-05-network-ids-integration/Fig_27.jpg)

![Verify the Suricata alerts in the Wazuh Dashboard](../assets/screenshots/poc-05-network-ids-integration/Fig_28.jpg)

![Verify the Suricata alerts in the Wazuh Dashboard](../assets/screenshots/poc-05-network-ids-integration/Fig_29.jpg)

![Verify the Suricata alerts in the Wazuh Dashboard](../assets/screenshots/poc-05-network-ids-integration/Fig_30.jpg)

![Verify the Suricata alerts in the Wazuh Dashboard](../assets/screenshots/poc-05-network-ids-integration/Fig_31.jpg)

---

# Next Step

Continue with the next Proof of Concept.

[06 - Detecting an SQL Injection Attack](06-detecting-an-sql-injection-attack.md)
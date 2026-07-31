# Apply Patch 4.14.6

---

# Table of Contents

1. Overview
2. Prepare the Repository
3. Stop the Services
4. Backup the Indexer Security Configuration
5. Disable Cluster Allocation
6. Flush the Indexer
7. Update the Wazuh Indexer
8. Initialize the Indexer Security Configuration
9. Re-enable Cluster Allocation
10. Update the Wazuh Manager
11. Update Filebeat
12. Update the Wazuh Dashboard
13. Disable Future Updates
14. Verify the Installed Version
15. Update Windows Agent
16. Update Linux Agent

---

## 1 - Overview

This guide describes how to update a distributed Wazuh environment to version **4.14.6**.

The update process must be performed on the following nodes:

- Wazuh Indexer
- Wazuh Server
- Wazuh Dashboard

---

## 2 - Prepare the Repository

Perform the following steps on the **Indexer**, **Server**, and **Dashboard** nodes.

Install the required packages.

```bash
sudo apt-get install gnupg apt-transport-https
```

Import the Wazuh GPG key.

```bash
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | gpg --no-default-keyring --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import && sudo chmod 644 /usr/share/keyrings/wazuh.gpg
```

![Install the required packages](../assets/screenshots/06-apply-patch/1.png)

Open the Wazuh repository configuration file.

```bash
sudo nano /etc/apt/sources.list.d/wazuh.list
```

Uncomment the repository entry.

Save the file and exit Nano.

```text
Ctrl + X
Y
Enter
```

![Uncomment repository entry](../assets/screenshots/06-apply-patch/2.png)

Update the package list.

```bash
sudo apt-get update
```

---

## 3 - Stop the Services

On the **Wazuh Server**, stop the Filebeat service.

```bash
sudo systemctl stop filebeat
```

On the **Wazuh Dashboard**, stop the Dashboard service.

```bash
sudo systemctl stop wazuh-dashboard
```

![Stop the services](../assets/screenshots/06-apply-patch/3.png)

---

## 4 - Backup the Indexer Security Configuration

On the **Wazuh Indexer**, create a backup of the security configuration.

This backup includes:

- Users
- Roles
- Permissions

```bash
sudo /usr/share/wazuh-indexer/bin/indexer-security-init.sh --options "-backup /etc/wazuh-indexer/opensearch-security -icl -nhnv"
```

![Backup the Indexer security configuration](../assets/screenshots/06-apply-patch/4.png)

---

## 5 - Disable Cluster Allocation

Temporarily allow only primary shard allocation before upgrading the Indexer.

```bash
curl -X PUT "https://<IP_INDEXER>:9200/_cluster/settings" -u admin -k -H 'Content-Type: application/json' -d'
{
  "persistent": {
    "cluster.routing.allocation.enable": "primaries"
  }
}
'
```

![Disable cluster allocation](../assets/screenshots/06-apply-patch/5.png)

---

## 6 - Flush the Indexer

Flush all pending data from memory to disk before stopping the Indexer.

```bash
curl -X POST "https://<IP_INDEXER>:9200/_flush" -u admin -k
```

![Flush the Indexer](../assets/screenshots/06-apply-patch/6.png)

---

## 7 - Update the Wazuh Indexer

On the **Wazuh Server**, stop the Wazuh Manager service.

```bash
sudo systemctl stop wazuh-manager
```

On the **Wazuh Indexer**, stop the Indexer service.

```bash
sudo systemctl stop wazuh-indexer
```

Create a backup of the `jvm.options` file before upgrading the Indexer.

```bash
sudo cp /etc/wazuh-indexer/jvm.options /etc/wazuh-indexer/jvm.options.old
```

Install the new Wazuh Indexer package.

```bash
sudo apt-get install wazuh-indexer
```

![Update the Wazuh Indexer](../assets/screenshots/06-apply-patch/7.png)

Restore the `jvm.options` configuration manually from the backup.

Compare the following files:

```text
compare

/etc/wazuh-indexer/jvm.options.old

and

/etc/wazuh-indexer/jvm.options
```

If both files are identical, no changes are required.

Reload the systemd configuration.

```bash
sudo systemctl daemon-reload
```

Enable the Wazuh Indexer service.

```bash
sudo systemctl enable wazuh-indexer
```

Start the Wazuh Indexer service.

```bash
sudo systemctl start wazuh-indexer
```

![Restore the JVM configuration](../assets/screenshots/06-apply-patch/8.png)

---

## 8 - Initialize the Indexer Security Configuration

Initialize the Indexer security configuration.

```bash
sudo /usr/share/wazuh-indexer/bin/indexer-security-init.sh
```

![Initialize the Indexer security configuration](../assets/screenshots/06-apply-patch/9.png)

Verify that the Indexer node is online.

```bash
curl -k -u admin https://<IP_INDEXER>:9200/_cat/nodes?v
```

![Verify the Indexer node](../assets/screenshots/06-apply-patch/10.png)

---

## 9 - Re-enable Cluster Allocation

Re-enable cluster allocation after the upgrade.

```bash
curl -X PUT "https://<IP_INDEXER>:9200/_cluster/settings" -u admin -k -H 'Content-Type: application/json' -d'
{
  "persistent": {
    "cluster.routing.allocation.enable": "all"
  }
}
'
```

![Re-enable cluster allocation](../assets/screenshots/06-apply-patch/11.png)

Verify that the cluster has returned to a healthy state.

```bash
curl -k -u admin https://<IP_INDEXER>:9200/_cat/nodes?v
```

![Verify the cluster status](../assets/screenshots/06-apply-patch/12.png)

---

## 10 - Update the Wazuh Manager

Install the Wazuh Manager update.

```bash
sudo apt-get install wazuh-manager
```

![Install the Wazuh Manager update](../assets/screenshots/06-apply-patch/13.png)

Reload the systemd configuration.

```bash
sudo systemctl daemon-reload
```

Enable the Wazuh Manager service.

```bash
sudo systemctl enable wazuh-manager
```

Start the Wazuh Manager service.

```bash
sudo systemctl start wazuh-manager
```

![Start the Wazuh Manager service](../assets/screenshots/06-apply-patch/14.png)

Verify that the `ossec.conf` file has not been modified.

```bash
sudo nano /var/ossec/etc/ossec.conf
```

Verify the following:

- The Indexer host is configured with the correct IP address.
- The certificate paths are correct.

---

## 11 - Update Filebeat

Download and extract the updated Wazuh Filebeat modules.

```bash
curl -s https://packages.wazuh.com/4.x/filebeat/wazuh-filebeat-0.5.tar.gz | sudo tar -xvz -C /usr/share/filebeat/module
```

![Download the updated Filebeat modules](../assets/screenshots/06-apply-patch/15.png)

---

Download the updated Wazuh indexer template.

```bash
curl -so /etc/filebeat/wazuh-template.json https://raw.githubusercontent.com/wazuh/wazuh/v4.14.6/extensions/elasticsearch/7.x/wazuh-template.json
```

Grant read permissions to the template.

```bash
sudo chmod go+r /etc/filebeat/wazuh-template.json
```

Backup the current Filebeat configuration.

```bash
sudo cp /etc/filebeat/filebeat.yml /etc/filebeat/filebeat.yml.old
```
![Backup filebeat](../assets/screenshots/06-apply-patch/16.png)

Install the Filebeat update.

```bash
sudo apt-get install filebeat
```

Reload the systemd configuration.

```bash
sudo systemctl daemon-reload
```

Enable the Filebeat service.

```bash
sudo systemctl enable filebeat
```

Start the Filebeat service.

```bash
sudo systemctl start filebeat
```
![Install filebeat](../assets/screenshots/06-apply-patch/17.png)

Configure the Filebeat pipelines.

```bash
sudo filebeat setup --pipelines
```

Configure index management.

```bash
sudo filebeat setup --index-management -E output.logstash.enabled=false
```
![Configure pipelines filebeat](../assets/screenshots/06-apply-patch/18.png)
---

## 12 - Update the Wazuh Dashboard

Backup the Wazuh Dashboard configuration.

```bash
sudo cp /etc/wazuh-dashboard/opensearch_dashboards.yml /etc/wazuh-dashboard/opensearch_dashboards.yml.old
```

Install the Wazuh Dashboard update.

```bash
sudo apt-get install wazuh-dashboard
```
![Backup Wazuh Dashboard](../assets/screenshots/06-apply-patch/19.png)

Verify the Dashboard configuration file.

```bash
sudo nano /etc/wazuh-dashboard/opensearch_dashboards.yml
```

Reload the systemd configuration.

```bash
sudo systemctl daemon-reload
```

Enable the Wazuh Dashboard service.

```bash
sudo systemctl enable wazuh-dashboard
```

Start the Wazuh Dashboard service.

```bash
sudo systemctl start wazuh-dashboard
```
![Verify Dashboard config file](../assets/screenshots/06-apply-patch/20.png)
---

## 13 - Disable Future Updates

Edit the Wazuh repository configuration file.

```bash
sudo nano /etc/apt/sources.list.d/wazuh.list
```
![Comment repository entry](../assets/screenshots/06-apply-patch/21.png)
Comment the Wazuh repository entry to prevent automatic updates.

---

## 14 - Verify the Installed Version

Verify the installed Wazuh Indexer version.

```bash
sudo apt list --installed wazuh-indexer
```

Verify the installed Wazuh Manager version.

```bash
sudo apt list --installed wazuh-manager
```

Verify the installed Wazuh Dashboard version.

```bash
sudo apt list --installed wazuh-dashboard
```
![Verify the Installed Version](../assets/screenshots/06-apply-patch/22.png)
![Verify the Installed Version on Dashboard](../assets/screenshots/06-apply-patch/23.png)

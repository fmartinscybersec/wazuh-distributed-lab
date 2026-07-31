# Wazuh Dashboard Installation

---

# Table of Contents

1. Requirements
2. Install Missing Packages (Part 1)
3. Install Missing Packages (Part 2)
4. Install the GPG Key
5. Add the Official Wazuh Repository
6. Update the Package List
7. Install the Wazuh Dashboard
8. Edit the `opensearch_dashboards.yml` File
9. Configure the Firewall Ports
10. Deploy the Certificates
11. Start the Service
12. Edit the `wazuh.yml` File
13. Disable Wazuh Updates

---

## 1 - Requirements

![Requirements](../assets/screenshots/03-wazuh-dashboard/Fig_1.jpg)

---

## 2 - Install Missing Packages (Part 1)

Run the following command to install the required packages if they are missing.

```bash
sudo apt-get install debhelper tar curl libcap2-bin
```

> **Note:** `debhelper` version 9 or later is required.

![Install missing packages (Part 1)](../assets/screenshots/03-wazuh-dashboard/Fig_2.jpg)

---

## 3 - Install Missing Packages (Part 2)

Run the following command to install the required packages if they are missing.

```bash
sudo apt-get install gnupg apt-transport-https
```

![Install missing packages (Part 2)](../assets/screenshots/03-wazuh-dashboard/Fig_3.jpg)

---

## 4 - Install the GPG Key

Download and import the official Wazuh GPG key.

```bash
sudo curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo gpg --no-default-keyring --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import && sudo chmod 644 /usr/share/keyrings/wazuh.gpg
```

![Install the GPG key](../assets/screenshots/03-wazuh-dashboard/Fig_4.jpg)

---

## 5 - Add the Official Wazuh Repository

Add the official Wazuh repository to the APT sources list.

```bash
echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" | sudo tee -a /etc/apt/sources.list.d/wazuh.list
```

![Add the official Wazuh repository](../assets/screenshots/03-wazuh-dashboard/Fig_5.jpg)

---

## 6 - Update the Package List

Update the package information.

```bash
sudo apt-get update
```

![Update the package list](../assets/screenshots/03-wazuh-dashboard/Fig_6.jpg)

---

## 7 - Install the Wazuh Dashboard

Install the Wazuh Dashboard package.

```bash
sudo apt-get -y install wazuh-dashboard
```

![Install the Wazuh Dashboard](../assets/screenshots/03-wazuh-dashboard/Fig_7.jpg)

---

## 8 - Edit the `opensearch_dashboards.yml` File

Open the Wazuh Dashboard configuration file.

```bash
sudo nano /etc/wazuh-dashboard/opensearch_dashboards.yml
```

![Open the Dashboard configuration file](../assets/screenshots/03-wazuh-dashboard/Fig_8.jpg)

Before editing:

![Default Dashboard configuration](../assets/screenshots/03-wazuh-dashboard/Fig_9.jpg)

Replace the default `opensearch.hosts` value (`localhost`) with the IP address of Wazuh Indexer.

Example:

```text
https://<IP_INDEXER>:9200
```

After editing:

![Updated Dashboard configuration](../assets/screenshots/03-wazuh-dashboard/Fig_10.jpg)

Save the file.

```text
Ctrl + X
Y
Enter
```

---

## 9 - Configure the Firewall Ports

Allow access to the Wazuh Dashboard Web User Interface (HTTPS).

```bash
sudo ufw allow from <YOUR_SUBNET>/24 to any port 443 proto tcp
```

Example:

```bash
sudo ufw allow from 192.168.232.0/24 to any port 443 proto tcp
```

![Allow HTTPS access](../assets/screenshots/03-wazuh-dashboard/Fig_11.png)

Verify that the firewall rule has been created successfully.

```bash
sudo ufw status verbose
```

![Verify the firewall configuration](../assets/screenshots/03-wazuh-dashboard/Fig_12.png)

---

## 10 - Deploy the Certificates

Verify that the `wazuh-certificates.tar` file is available.

```bash
ls
```

![Verify the certificate archive](../assets/screenshots/03-wazuh-dashboard/Fig_13.jpg)

Create the `certs` directory.

```bash
sudo mkdir /etc/wazuh-dashboard/certs
```

Verify that the directory has been created.

```bash
sudo ls /etc/wazuh-dashboard/
```

![Create the certificates directory](../assets/screenshots/03-wazuh-dashboard/Fig_14.jpg)

Extract the required certificates.

```bash
sudo tar -xf ./wazuh-certificates.tar \
-C /etc/wazuh-dashboard/certs/ \
./dashboard.pem \
./dashboard-key.pem \
./root-ca.pem
```

Verify that the certificates have been extracted.

```bash
sudo ls /etc/wazuh-dashboard/certs
```

![Extract the certificates](../assets/screenshots/03-wazuh-dashboard/Fig_15.jpg)

Enter a root shell.

```bash
sudo -i
```

![Enter root shell](../assets/screenshots/03-wazuh-dashboard/Fig_16.jpg)

Set the appropriate permissions.

```bash
sudo chmod 500 /etc/wazuh-dashboard/certs
```

```bash
sudo chmod 400 /etc/wazuh-dashboard/certs/*
```

```bash
sudo chown -R wazuh-dashboard:wazuh-dashboard /etc/wazuh-dashboard/certs
```

![Configure certificate permissions](../assets/screenshots/03-wazuh-dashboard/Fig_17.jpg)

Remove the compressed certificate archive.

```bash
sudo rm -f ./wazuh-certificates.tar
```

Verify that the archive has been removed.

```bash
ls
```

![Remove the certificate archive](../assets/screenshots/03-wazuh-dashboard/Fig_18.jpg)

---

## 11 - Start the Wazuh Dashboard Service

Reload the systemd daemon.

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

Verify the service status.

```bash
sudo systemctl status wazuh-dashboard
```

![Start the Wazuh Dashboard service](../assets/screenshots/03-wazuh-dashboard/Fig_19.jpg)

---

## 12 - Edit the `wazuh.yml` File

Open the Wazuh Dashboard plugin configuration file.

```bash
sudo nano /usr/share/wazuh-dashboard/data/wazuh/config/wazuh.yml
```

![Open the wazuh.yml file](../assets/screenshots/03-wazuh-dashboard/Fig_20.jpg)

Before editing:

![Default wazuh.yml configuration](../assets/screenshots/03-wazuh-dashboard/Fig_21.jpg)

Replace the default `url` host with the IP address of your Wazuh Server.

Example:

```yaml
hosts:
  - default:
      url: https://<IP_SERVER>
      port: 55000
      username: wazuh-wui
      password: wazuh-wui
      run_as: true
```

After editing:

![Updated wazuh.yml configuration](../assets/screenshots/03-wazuh-dashboard/Fig_22.jpg)

Save the file.

```text
Ctrl + X
Y
Enter
```

Restart the Wazuh Dashboard service.

```bash
sudo systemctl restart wazuh-dashboard
```

Verify that the service is running correctly.

```bash
sudo systemctl status wazuh-dashboard
```
---

## 13 - Disable Wazuh Updates

Disable the Wazuh repository to prevent automatic updates.

```bash
sudo sed -i "s/^deb /#deb /" /etc/apt/sources.list.d/wazuh.list
sudo apt update
```

![Disable Wazuh updates](../assets/screenshots/03-wazuh-dashboard/Fig_23.jpg)

---

# Next Step

Continue with the Wazuh password management.

[04 - Change Internal Passwords](04-password-management.md)
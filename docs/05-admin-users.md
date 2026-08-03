# Create Administrator Users

---

# Table of Contents

1. Overview
2. Open the Security Menu
3. Create an Internal User
4. Create a Role Mapping
5. Verify the `run_as` Setting
6. Restart the Wazuh Dashboard

---

## 1 - Overview

Create administrator users so everyone can test the platform.

These users are administrators but do not have permissions related to the security rules.

The objective is to allow everyone to test the platform and report any missing permissions so they can be added later.

In the future, access should be based on roles similar to the following:

- Administrator – full platform management.
- SOC Manager / Team Leader – operational management and validation.
- SOC Analyst – alert and incident investigation.
- Threat Intelligence – access to MISP and indicators.
- Read Only / Auditor – view-only access and reporting.

This follows the Principle of Least Privilege, which is a security best practice.

---

## 2 - Open the Security Menu

Navigate to:

**Indexer Management → Security**

![Open the Security menu](../assets/screenshots/05-admin-users/1.png)

---

## 3 - Create an Internal User

Inside **Security**, select:

**Internal Users → Create Internal User**

![Open the Internal Users page](../assets/screenshots/05-admin-users/2.png)

Create a new internal user.

Configure:

- Login
- Password
- Backend role: `admin`

Click **Create**.

![Create an internal user](../assets/screenshots/05-admin-users/3.png)

---

## 4 - Create a Role Mapping

Navigate to:

**Server Management → Security**

![Open the Server Management Security menu](../assets/screenshots/05-admin-users/4.png)

Inside **Security**, select:

**Roles Mapping → Create Role Mapping**

![Open the Roles Mapping page](../assets/screenshots/05-admin-users/5.png)

Create a new role mapping.

Configure:

- Name
- Role: `administrator`
- Internal users: Select the internal user created in the previous step.

Click **Create**.

![Create a role mapping](../assets/screenshots/05-admin-users/6.png)

---

## 5 - Verify the `run_as` Setting

On the Wazuh Dashboard server, verify that the `run_as` parameter is set to `true` at the end of the `wazuh.yml` configuration file.

```bash
sudo nano /usr/share/wazuh-dashboard/data/wazuh/config/wazuh.yml
```

![Open the wazuh.yml file](../assets/screenshots/05-admin-users/7.png)

Verify that the following setting is present.

```yaml
run_as: true
```

---

## 6 - Restart the Wazuh Dashboard

Restart the Wazuh Dashboard service and verify its status.

```bash
sudo systemctl restart wazuh-dashboard
```

```bash
sudo systemctl status wazuh-dashboard
```

![Restart and verify the Wazuh Dashboard service](../assets/screenshots/05-admin-users/8.png)

---

# Next Step

Continue with the Wazuh patch installation.

[06 - Apply Patch](06-apply-patch.md)
# Wazuh Installation on Ubuntu Server 22.04 LTS

This repository contains the complete step-by-step process of installing and configuring Wazuh SIEM on an Ubuntu Server 22.04 LTS environment using VMware.

---

# Environment Information

| Component | Details |
|---|---|
| Operating System | Ubuntu Server 22.04 LTS |
| Virtualization | VMware |
| Wazuh Version | 4.7.5 |
| Server IP | 192.168.1.4 |
| Agent OS | Windows 11 |
| Dashboard Port | 443 |

---

# Step 1: Login to Ubuntu Server

Connect to the Ubuntu Server using VMware console or PuTTY.

Example login screen:

```bash
login as: mahafuzborshon
mahafuzborshon@192.168.1.4's password:
```

After successful login:

```bash
Welcome to Ubuntu 22.04.5 LTS
```

---

# Step 2: Update and Upgrade Packages

Switch to root user:

```bash
sudo su
```

Update package repositories and upgrade installed packages:

```bash
apt update && apt upgrade -y
```

Expected output:

```bash
All packages are up to date.
```

---

# Step 3: Install Required Dependencies

Install required packages for Wazuh installation:

```bash
sudo apt install curl apt-transport-https unzip wget gnupg -y
```

These packages are required for:

- Downloading files
- Managing repositories
- Extracting archives
- Package authentication

---

# Step 4: Create Swap Memory (Important)

Wazuh Indexer requires additional memory. Create a 4GB swap file.

Create swap file:

```bash
sudo fallocate -l 4G /swapfile
```

Set correct permissions:

```bash
sudo chmod 600 /swapfile
```

Configure swap area:

```bash
sudo mkswap /swapfile
```

Enable swap:

```bash
sudo swapon /swapfile
```

Verify swap:

```bash
free -h
```

---

# Step 5: Download Wazuh Installation Script

Download the official Wazuh installation assistant:

```bash
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
```

Give executable permission:

```bash
chmod +x wazuh-install.sh
```

---

# Step 6: Install Wazuh Platform

Run the installation script:

```bash
sudo bash wazuh-install.sh -a
```

The installation assistant installs:

- Wazuh Manager
- Wazuh Indexer
- Wazuh Dashboard

---

# Step 7: Fix Existing Installation Conflict (If Needed)

If you see:

```bash
ERROR: Wazuh manager already installed.
ERROR: Wazuh indexer already installed.
ERROR: Wazuh dashboard already installed.
```

Run:

```bash
sudo bash wazuh-install.sh -a -o
```

The `-o` flag overwrites the previous installation.

---

# Step 8: Installation Process

During installation, the script:

- Removes previous installations
- Adds Wazuh repository
- Generates configuration files
- Creates certificates
- Installs Wazuh components
- Configures OpenSearch security

Example logs:

```bash
INFO: Wazuh indexer installation finished.
INFO: Wazuh dashboard removed.
INFO: Starting Wazuh indexer installation.
```

---

# Step 9: Enable and Start Wazuh Manager

Enable Wazuh service:

```bash
sudo systemctl enable wazuh-manager
```

Start Wazuh Manager:

```bash
sudo systemctl start wazuh-manager
```

Check status:

```bash
sudo systemctl status wazuh-manager
```

Expected status:

```bash
Active: active (running)
```

---

# Step 10: Generate Wazuh Password Hash

Run:

```bash
sudo /usr/share/wazuh-indexer/plugins/opensearch-security/tools/hash.sh
```

Example:

```bash
[Password:]
$2y$12$...
```

This hash can be used for authentication configuration if required.

---

# Step 11: Verify Installed Packages

Check installed Wazuh components:

```bash
dpkg -l | grep wazuh
```

Expected output:

```bash
ii  wazuh-dashboard
ii  wazuh-indexer
ii  wazuh-manager
```

---

# Step 12: Access Wazuh Dashboard

Open browser:

```text
https://192.168.1.4
```

Default credentials:

```text
Username: admin
Password: admin
```

> Change the default password after first login.

---

# Step 13: Deploy Wazuh Agent on Windows

Install Wazuh agent on Windows machine.

After installation:

- Connect the agent to Wazuh Manager
- Verify the agent status from Dashboard

Expected status:

```text
Active
```

---

# Step 14: Verify Agent Connection

From Wazuh Dashboard:

Navigate to:

```text
Agents → Overview
```

Check:

- Agent name
- IP address
- Operating system
- Status = Active

---

# Important Ports

| Port | Purpose |
|---|---|
| 1514 | Agent communication |
| 1515 | Agent enrollment |
| 443 | Dashboard access |

Allow these ports through firewall:

```bash
sudo ufw allow 1514/tcp
sudo ufw allow 1515/tcp
sudo ufw allow 443/tcp
```

Reload firewall:

```bash
sudo ufw reload
```

---

# Useful Commands

## Check Wazuh Services

```bash
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-dashboard
```

---

## Restart Services

```bash
sudo systemctl restart wazuh-manager
sudo systemctl restart wazuh-indexer
sudo systemctl restart wazuh-dashboard
```

---

## Check Listening Ports

```bash
sudo ss -tulnp
```

---

## Check Logs

Manager logs:

```bash
sudo tail -f /var/ossec/logs/ossec.log
```

Installation logs:

```bash
sudo tail -f /var/log/wazuh-install.log
```

---

# Final Verification Checklist

| Task | Status |
|---|---|
| Ubuntu updated | ✅ |
| Dependencies installed | ✅ |
| Swap memory configured | ✅ |
| Wazuh installed | ✅ |
| Wazuh Manager active | ✅ |
| Dashboard accessible | ✅ |
| Windows agent connected | ✅ |

---

# Conclusion

Wazuh SIEM was successfully installed and configured on Ubuntu Server 22.04 LTS. The Wazuh Dashboard is operational, and Windows agents are actively connected to the manager for monitoring and security event collection.

This setup provides:

- Centralized log management
- Threat detection
- Security monitoring
- File integrity monitoring
- Endpoint visibility

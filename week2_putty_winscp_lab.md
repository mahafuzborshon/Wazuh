# Week 2 Lab: Setting Up PuTTY and WinSCP for Remote Ubuntu Access

## What You’ll Learn
By the end of this lab, you’ll be able to:

- Install PuTTY and WinSCP on a Windows computer
- Connect to your Ubuntu virtual machine using SSH
- Transfer files between Windows and Ubuntu securely
- Save connection profiles for quicker access later
- Access and manage Ubuntu files remotely from Windows

---

# Lab Goal
In this lab, you’ll configure two essential remote-access tools:

- **PuTTY** for terminal access
- **WinSCP** for file transfers

These tools will help you manage your Ubuntu server more efficiently while working on your Wazuh setup.

---

# Scenario
Your Ubuntu VM is running separately from your Windows machine, and you want a faster way to manage it without constantly switching screens. As part of a SOC or cybersecurity environment, you’ll often need:

- Secure remote terminal access
- Fast file transfers
- Easy management of logs, scripts, and configuration files

PuTTY gives you command-line access through SSH, while WinSCP provides a graphical interface for transferring files.

---

# Requirements
Before starting, make sure you have:

- A Windows 10 or Windows 11 system
- Your Ubuntu VM running
- Ubuntu username and password
- Ubuntu VM IP address
- SSH service enabled on Ubuntu

To verify SSH is running, use:

```bash
sudo systemctl status ssh
```

---

# Part 1: Installing PuTTY

## Step 1: Download PuTTY

1. Open your browser on Windows.
2. Visit the official website:

   https://www.putty.org/

3. Click **Download PuTTY**.
4. Download the **64-bit MSI installer**.

> Using the installer version is recommended because it automatically adds shortcuts and required files.

---

## Step 2: Install PuTTY

1. Open the downloaded installer.
2. Click **Next**.
3. Accept the license agreement.
4. Continue with the default installation settings.
5. Click **Install**.
6. Once installation finishes, click **Finish**.

---

## Step 3: Open PuTTY

- Open the Start Menu
- Search for **PuTTY**
- Launch the application

---

# Part 2: Connecting to Ubuntu with PuTTY

## Step 1: Find Your Ubuntu IP Address

Inside Ubuntu, run:

```bash
ip a
```

Find the IP address listed under your network interface.

Example:

```bash
192.168.1.100
```

Keep this address available because you’ll use it to connect from Windows.

---

## Step 2: Configure the SSH Session

Inside PuTTY:

| Setting | Value |
|---|---|
| Host Name | Your Ubuntu IP address |
| Port | 22 |
| Connection Type | SSH |

### Optional: Save the Session

1. In **Saved Sessions**, type:

```text
Ubuntu-Wazuh
```

2. Click **Save**.

This allows you to reconnect quickly later.

---

## Step 3: Connect to Ubuntu

1. Click **Open**.
2. A security warning may appear the first time.
3. Click **Accept**.

Now enter:

- Ubuntu username
- Ubuntu password

> Your password will not appear while typing. This is normal behavior in Linux terminals.

---

## Step 4: Confirm the Connection

If the login is successful, you should see something similar to:

```bash
wazuh-user@ubuntu:~$
```

Run these commands to test:

```bash
whoami
```

```bash
pwd
```

Expected output:

```bash
wazuh-user
/home/wazuh-user
```

At this point, your Windows machine is successfully controlling the Ubuntu VM remotely.

---

# Part 3: Installing WinSCP

## Step 1: Download WinSCP

1. Open your browser.
2. Visit:

   https://winscp.net/

3. Download the latest installer version.

---

## Step 2: Install WinSCP

1. Launch the installer.
2. Choose **Typical Installation**.
3. Select the **Commander Interface**.
4. Click **Install**.
5. Finish the installation.

---

# Part 4: Using WinSCP for File Transfers

## Step 1: Create a New Connection

Open WinSCP and enter the following:

| Setting | Value |
|---|---|
| File Protocol | SCP or SFTP |
| Host Name | Ubuntu IP address |
| Port Number | 22 |
| User Name | Ubuntu username |
| Password | Ubuntu password |

---

## Step 2: Save the Connection

1. Click **Save**.
2. Name the session:

```text
Ubuntu-Wazuh
```

3. Optionally enable **Save Password**.
4. Click **OK**.

---

## Step 3: Connect

1. Click **Login**.
2. Accept the SSH security prompt if it appears.

Once connected, WinSCP will display two panels.

| Panel | Purpose |
|---|---|
| Left | Files on your Windows PC |
| Right | Files on the Ubuntu VM |

---

## Step 4: Transfer a Test File

### On Windows:

1. Create a text file named:

```text
test.txt
```

2. Drag the file from the left panel to the right panel.

### On Ubuntu:

Use PuTTY and run:

```bash
ls -la
```

You should see:

```bash
test.txt
```

This confirms file transfers are working correctly.

---

# Part 5: Helpful PuTTY Settings

## Save Session Logs

1. In PuTTY, go to:

```text
Category → Logging
```

2. Select:

```text
All session output
```

3. Choose a location for the log file.
4. Save the session.

---

## Increase Font Size

1. Navigate to:

```text
Window → Appearance
```

2. Click **Change** under font settings.
3. Select a larger font size.

---

## Copy and Paste in PuTTY

| Action | Method |
|---|---|
| Copy Text | Highlight with mouse |
| Paste Text | Right-click inside PuTTY |

---

# Troubleshooting

| Problem | Solution |
|---|---|
| Connection refused | Start SSH using `sudo systemctl enable ssh --now` |
| Host not found | Double-check the Ubuntu IP address |
| Login keeps failing | Verify username and password |
| WinSCP authentication failed | Make sure SSH is running |
| Cannot ping Ubuntu | Check VM network settings |
| Permission denied in WinSCP | Upload files inside your home directory |

---

# Useful Linux Commands

```bash
# Show logged-in username
whoami

# Display current directory
pwd

# List files
ls -la

# Create a folder
mkdir test-folder

# Delete a file
rm test.txt

# Display IP address
ip a | grep inet

# Restart SSH
sudo systemctl restart ssh
```

---

# Final Summary

| Tool | Purpose |
|---|---|
| PuTTY | Remote terminal access through SSH |
| WinSCP | Secure file transfer between Windows and Ubuntu |

Both tools are commonly used in Linux administration, cybersecurity, and SOC environments. Once configured, they make managing your Ubuntu server much faster and easier from Windows.

# Getting Started
## Lab 02 – Shell Scripting and Linux Automation

## Overview

This lab evaluates Linux shell scripting, automation, system administration, reporting, and operational monitoring skills. Students will develop Bash scripts to automate common administrative tasks including environment reporting, user provisioning, disk monitoring, backup operations, and reusable system administration functions.

> **Important:**
>
> * You are restricted to **CLI-based administration only**.
> * All tasks must be completed using **Bash shell scripts**.
> * Script filenames must match the names specified in each task.

## Access Information

You will perform all lab tasks on a Linux virtual machine provisioned for this environment.

### SSH Connection

Use the following command to connect to the virtual machine:

```bash
ssh <inject key="VMUserName" enableCopy="true"/>@<inject key="VMPublicDNSName" enableCopy="true"/>
```

### Username

```text
<inject key="VMUserName" enableCopy="true"/>
```

### Password

```text
<inject key="VMPassword" enableCopy="true"/>
```

## Note

If you prefer not to use a browser-based terminal, you may connect to the virtual machine directly from your local computer using any SSH client, including:

* OpenSSH (Linux/macOS)
* Windows Terminal or PowerShell
* PuTTY
* MobaXterm
* Termius

Use the SSH command and credentials provided above to establish the connection.

> **Important:** Ensure that your local network allows outbound SSH connections on port 22 before attempting to connect from your local machine.

## Verification

After connecting successfully, verify access by running:

```bash
whoami
hostname
```

The commands should display your current username and the hostname of the lab virtual machine.

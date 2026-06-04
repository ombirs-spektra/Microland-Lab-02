**CloudLabs by Spektra Systems** | Facilitator Solution Guide (NOT for candidates)

# Lab 02 - Shell Scripting Basics: Answer Key + Walkthrough

This document mirrors the candidate lab guide order. Each task includes the recommended implementation approach, validation expectations, key commands, expected artifacts.

---

# Task 1: Create Environment Reporting Utility

## Objective

Create a reusable Bash script that collects system information using environment variables and generates a report.

## Solution

Create:

```bash
touch env-report.sh
chmod +x env-report.sh
nano env-report.sh
```

```bash
#!/bin/bash

REPORT_DIR="/home/labuser/reports"
REPORT_FILE="$REPORT_DIR/EnvironmentReport.txt"

mkdir -p "$REPORT_DIR"

echo "Username: $USER" > "$REPORT_FILE"
echo "Hostname: $(hostname)" >> "$REPORT_FILE"
echo "Home Directory: $HOME" >> "$REPORT_FILE"
echo "Shell: $SHELL" >> "$REPORT_FILE"
echo "Current Path: $(pwd)" >> "$REPORT_FILE"

echo "Environment report generated."
```

Execute:

```bash
./env-report.sh
```

## Expected Output

```text
Username: labuser
Hostname: ip-10-10-0-25
Home Directory: /home/labuser
Shell: /bin/bash
Current Path: /home/labuser
```

## Validation Expectations

Verify:

```bash
ls /home/labuser/reports/EnvironmentReport.txt
```

Verify content:

```bash
cat /home/labuser/reports/EnvironmentReport.txt
```


# Task 2: Automate User Provisioning

## Objective

Automate Linux user creation while handling duplicate accounts and generating a report.

## Solution

Create:

```bash
touch create-users.sh
chmod +x create-users.sh
nano create-users.sh
```

```bash
#!/bin/bash

REPORT="/home/labuser/reports/UserCreationReport.txt"

mkdir -p /home/labuser/reports

for USERNAME in employee1 employee2 employee3
do

    if id "$USERNAME" >/dev/null 2>&1
    then
        STATUS="Already Exists"
    else
        sudo useradd "$USERNAME"
        echo "$USERNAME:P@ssw0rd123!" | sudo chpasswd
        sudo chage -M 99999 "$USERNAME"
        STATUS="Created"
    fi

    echo "$USERNAME | $STATUS | $(date)" >> "$REPORT"

done
```

Execute:

```bash
chmod +x create-users.sh
./create-users.sh
```

## Validation

Verify users:

```bash
id employee1
id employee2
id employee3
```

Verify report:

```bash
cat /home/labuser/reports/UserCreationReport.txt
```

## Expected Output

```text
employee1 | Created | Tue Jun 2 10:20:10 UTC 2026
employee2 | Created | Tue Jun 2 10:20:10 UTC 2026
employee3 | Created | Tue Jun 2 10:20:10 UTC 2026
```

# Task 3: Build Disk Monitoring Utility

## Objective

Monitor filesystem capacity and generate alerts when free space drops below threshold.

## Solution

Create:

```bash
touch disk-alert.sh
chmod +x disk-alert.sh
nano disk-alert.sh
```

```bash
#!/bin/bash

REPORT="/home/labuser/reports/DiskUsageLog.txt"
THRESHOLD=20

mkdir -p /home/labuser/reports

FILESYSTEM=$(df -h / | awk 'NR==2 {print $1}')
TOTAL_SIZE=$(df -BG / | awk 'NR==2 {gsub("G","",$2); print $2}')
FREE_SPACE=$(df -BG / | awk 'NR==2 {gsub("G","",$4); print $4}')
FREE_PERCENT=$(df / | awk 'NR==2 {print 100-$5}')

if [ "$FREE_PERCENT" -lt "$THRESHOLD" ]
then
    STATUS="WARNING: Disk space below threshold."
else
    STATUS="Disk space is healthy."
fi

{
    echo "Filesystem: $FILESYSTEM"
    echo "Total Size (GB): $TOTAL_SIZE"
    echo "Free Space (GB): $FREE_SPACE"
    echo "Free Percentage: $FREE_PERCENT%"
    echo "Status: $STATUS"
    echo "Date and Time: $(date '+%Y-%m-%d %H:%M:%S')"
} > "$REPORT"

echo "$STATUS"
```

Execute:

```bash
chmod +x disk-alert.sh
./disk-alert.sh
```

## Validation

```bash
cat /home/labuser/reports/DiskUsageLog.txt
```
---

# Task 4: Create Automated Backup Solution

## Objective

Generate timestamped compressed backups and maintain backup records.

## Solution

Create:

```bash
mkdir -p /home/labuser/LabData
echo "Sample File 1" > /home/labuser/LabData/file1.txt

echo "Sample File 2" > /home/labuser/LabData/file2.txt

echo "Sample File 3" > /home/labuser/LabData/file3.txt

touch backup-folder.sh
chmod +x backup-folder.sh
nano backup-folder.sh
```

```bash
#!/bin/bash

SOURCE="/home/labuser/LabData"
BACKUP_DIR="/home/labuser/backups"
REPORT="/home/labuser/reports/BackupLog.txt"

mkdir -p "$BACKUP_DIR"
mkdir -p /home/labuser/reports

TIMESTAMP=$(date +%Y%m%d-%H%M%S)

BACKUP_FILE="$BACKUP_DIR/backup-$TIMESTAMP.tar.gz"

tar -czf "$BACKUP_FILE" "$SOURCE"

echo "Backup Time: $(date)" > "$REPORT"
echo "Source Path: $SOURCE" >> "$REPORT"
echo "Backup File: $BACKUP_FILE" >> "$REPORT"

echo "Backup completed."
```

Execute:

```bash
chmod +x backup-folder.sh
./backup-folder.sh
```

## Validation

Verify archive:

```bash
ls /home/labuser/backups
```

Verify contents:

```bash
tar -tzf <backupfile>
```
---

# Task 5: Build Reusable System Administration Toolkit

## Objective

Create reusable Bash functions for future automation tasks.

## Solution

Create:

```bash
touch system-tools.sh
chmod +x system-tools.sh
nano system-tools.sh
```

```bash
#!/bin/bash

REPORT="/home/labuser/reports/SystemInfoReport.txt"

mkdir -p /home/labuser/reports

get_system_info() {

    echo "Hostname: $(hostname)"

    echo "Operating System: $(grep PRETTY_NAME /etc/os-release | cut -d= -f2 | tr -d '\"')"

    echo "Current User: $(whoami)"
}

get_disk_status() {

    MOUNT_POINT=${1:-/}

    FILESYSTEM=$(df -h "$MOUNT_POINT" | awk 'NR==2 {print $1}')
    TOTAL=$(df -h "$MOUNT_POINT" | awk 'NR==2 {print $2}')
    AVAILABLE=$(df -h "$MOUNT_POINT" | awk 'NR==2 {print $4}')

    echo "Filesystem: $FILESYSTEM"
    echo "Total Space: $TOTAL"
    echo "Available Space: $AVAILABLE"
}

{
    get_system_info
    echo
    get_disk_status /
} | tee "$REPORT"
```

Execute:

```bash
chmod +x system-tools.sh
./system-tools.sh
```

## Validation

Verify:

```bash
cat /home/labuser/reports/SystemInfoReport.txt
```

---

# Final Assessment Validation

Expected Artifacts:

```text
/home/labuser/env-report.sh
/home/labuser/create-users.sh
/home/labuser/disk-alert.sh
/home/labuser/backup-folder.sh
/home/labuser/system-tools.sh

/home/labuser/reports/EnvironmentReport.txt
/home/labuser/reports/UserCreationReport.txt
/home/labuser/reports/DiskUsageLog.txt
/home/labuser/reports/BackupLog.txt
/home/labuser/reports/SystemInfoReport.txt
```

Expected System State:

```text
employee1 exists
employee2 exists
employee3 exists

At least one .tar.gz archive exists
```

---

Lab Completed Successfully.

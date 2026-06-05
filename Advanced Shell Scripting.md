# Advanced Shell Scripting Project

## Introduction

Shell scripting is a powerful tool used by Linux system administrators and DevOps engineers to automate repetitive tasks and improve system management efficiency. In this project, a Bash script was developed to automate system health monitoring by collecting information about CPU usage, memory usage, disk usage, and running processes. The script was further enhanced by scheduling automatic execution using Cron and implementing email notification functionality for system alerts.

The project demonstrates practical Linux administration skills and highlights the importance of automation in maintaining reliable and efficient systems.

---

# Project Objectives

The objectives of this project are:

* Create a Bash script to monitor system health.
* Check CPU, memory, and disk utilization.
* Display the top running processes.
* Automate script execution using Cron.
* Configure email alerts for high CPU usage.
* Gain hands-on experience with Linux automation tools.

---

# Environment

| Component        | Description                |
| ---------------- | -------------------------- |
| Operating System | Ubuntu Server (Vagrant VM) |
| Shell            | Bash                       |
| Scheduler        | Cron                       |
| Mail Service     | Postfix                    |
| Mail Utility     | Mailutils                  |

---

# Step 1: Create the Health Check Script

Create the script file:

```bash
nano health_check.sh
```

Add the following content:

```bash
#!/bin/bash

echo "=============================="
echo " SYSTEM HEALTH CHECK REPORT "
echo " Generated: $(date)"
echo "=============================="

echo ""
echo "CPU Usage:"
top -bn1 | grep "load average"

echo ""
echo "Memory Usage:"
free -m

echo ""
echo "Disk Usage:"
df -h /

echo ""
echo "Top 5 CPU Consuming Processes:"
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu | head -n 6
```

---

## Screenshot 1: Script Creation

<img width="812" height="54" alt="image" src="https://github.com/user-attachments/assets/611b4a62-d38f-435c-b8ef-1d57722d4c8e" />

<img width="1278" height="1440" alt="image" src="https://github.com/user-attachments/assets/3b880928-3bbd-4308-9bb1-579c08e26fb3" />


---

# Step 2: Make the Script Executable

Run:

```bash
chmod +x health_check.sh
```

Verify:

```bash
ls -l health_check.sh
```

---

## Screenshot 2: Script Permissions

<img width="926" height="48" alt="image" src="https://github.com/user-attachments/assets/54e6ee76-6505-47b3-a752-12665fc2ef66" />

<img width="1226" height="150" alt="image" src="https://github.com/user-attachments/assets/1e8918f1-07cc-4b86-8903-7fcbac5df86c" />

---

# Step 3: Execute the Script

Run:

```bash
./health_check.sh
```

Example output:

```text
==============================
 SYSTEM HEALTH CHECK REPORT
 Generated: Wed Jun 3 15:00:00 UTC 2026
==============================

CPU Usage:
load average: 0.05, 0.08, 0.10

Memory Usage:
              total        used        free
Mem:           1970         350        1200

Disk Usage:
Filesystem      Size Used Avail Use%
/dev/sda1        25G  4G   20G  18%
```

---

## Screenshot 3: Script Output

<img width="1228" height="1034" alt="image" src="https://github.com/user-attachments/assets/1cf7c1e7-b6fd-49c1-b79f-42b047368239" />

---

# Step 4: Schedule the Script Using Cron

Edit Cron jobs:

```bash
crontab -e
```

Add:

```bash
0 * * * * /home/vagrant/health_check.sh >> /home/vagrant/health_check.log 2>&1
```

This configuration runs the script every hour and stores the output in a log file.

---

## Screenshot 4: Cron Configuration

<img width="654" height="62" alt="image" src="https://github.com/user-attachments/assets/4210cb35-8d86-46e0-90d2-e0cc636a02a7" />

<img width="1126" height="392" alt="image" src="https://github.com/user-attachments/assets/2444164c-f26d-43b8-8562-ef4bb43e275c" />

<img width="1294" height="1440" alt="image" src="https://github.com/user-attachments/assets/eb1c6b19-7154-4a0a-bcd3-96a64341aa14" />

<img width="1272" height="1440" alt="image" src="https://github.com/user-attachments/assets/dccaa757-ee25-4d9c-a787-90d3ad74820d" />

<img width="1096" height="198" alt="image" src="https://github.com/user-attachments/assets/8b9b069e-5322-4190-9284-fc5bd12f1ef2" />

---

# Step 5: Verify Cron Configuration

Display the active cron jobs:

```bash
crontab -l
```

Expected output:

```text
0 * * * * /home/vagrant/health_check.sh >> /home/vagrant/health_check.log 2>&1
```

---

## Screenshot 5: Cron Verification

<img width="1248" height="1068" alt="image" src="https://github.com/user-attachments/assets/73e16326-7723-41c9-8e04-ce7456617720" />

---

# Step 6: Verify Log File Generation

Check the generated log file:

```bash
cat health_check.log
```

or

```bash
tail health_check.log
```

---

## Screenshot 6: Log File Output

<img width="1258" height="1092" alt="image" src="https://github.com/user-attachments/assets/db8dcd90-3b13-43c4-a900-f9b14f5a922f" />

---

# Step 7: Configure Email Notifications

Install Mailutils:

```bash
sudo apt install mailutils -y
```

Install Postfix:

```bash
sudo apt install postfix -y
```

Select:

* Internet Site
* Default hostname

---

## Screenshot 7: Mailutils Installation

<img width="1268" height="1440" alt="image" src="https://github.com/user-attachments/assets/b6bf0a9f-db58-4b66-aa68-2a7f9c86967f" />

<img width="1266" height="1070" alt="image" src="https://github.com/user-attachments/assets/9fd52fab-c0f8-413f-b005-95a42dfc8420" />

<img width="1272" height="1440" alt="image" src="https://github.com/user-attachments/assets/40e822c5-79f6-41d4-8359-7877a97f3d14" />

<img width="1214" height="346" alt="image" src="https://github.com/user-attachments/assets/12ad453b-ab44-41e8-9a93-56c406dd75c3" />

---

# Step 8: Add CPU Alert Logic

Modify the script:

```bash
#!/bin/bash

CPU_USAGE=$(top -bn1 | grep load | awk '{print $10}' | sed 's/,//')

if (( $(echo "$CPU_USAGE > 0.90" | bc -l) ))
then
    echo "High CPU Usage Detected: $CPU_USAGE" | mail -s "CPU ALERT" tunmise216@gmail.com
fi
```

---

## Screenshot 8: Email Alert Configuration

<img width="782" height="66" alt="image" src="https://github.com/user-attachments/assets/45fff2e8-72e9-4a36-b600-7e93757ceb5f" />

<img width="1274" height="1440" alt="image" src="https://github.com/user-attachments/assets/84cf822b-c5cb-428a-993d-6865a28e26e0" />

---

# Step 9: Verify Postfix Service

Check Postfix status:

```bash
systemctl status postfix
```

Output should indicate that the service is installed and active.

---

## Screenshot 9: Postfix Status

<img width="1274" height="1440" alt="image" src="https://github.com/user-attachments/assets/e5b5eebe-86f7-43a0-a24f-a976c9ab6421" />

---

# Challenges Encountered

During the implementation, email messages generated by the script were successfully processed by Postfix but could not be delivered to external Gmail addresses. Investigation of the mail logs revealed that the local mail server was not configured with an external SMTP relay. Despite this limitation, the email alert functionality and mail processing workflow were successfully verified.

---

# Key Learning Outcomes

* Linux shell scripting
* Process monitoring
* Memory and disk monitoring
* Cron job scheduling
* Log management
* Email notification systems
* Linux troubleshooting and debugging

---

# Conclusion

This project successfully demonstrated the use of advanced shell scripting techniques for Linux system administration. A Bash script was created to monitor system resources and generate health reports. Automation was implemented using Cron, while Mailutils and Postfix were configured to support alert notifications. The project provided valuable hands-on experience in system monitoring, automation, and troubleshooting, which are essential skills for DevOps and Linux administrators.

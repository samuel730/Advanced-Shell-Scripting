# Advanced Shell Scripting Project

## Project Overview

This project demonstrates advanced Linux shell scripting skills for system administration. The script automates system health monitoring by checking CPU usage, memory usage, disk usage, and running processes. It also integrates cron scheduling and email notification features for system alerts.

---

## Objectives

* Write a shell script for system health monitoring
* Automate system checks (CPU, memory, disk, processes)
* Schedule script execution using cron
* Implement email alert notifications for high CPU usage
* Practice Linux system administration and automation

---

## System Health Check Script

### Script: `health_check.sh`
```bash
nano health_check.sh
```

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

## Make Script Executable

```bash
chmod +x health_check.sh
```

Verify permissions:

```bash
ls -l health_check.sh
```

---

## Run the Script

```bash
./health_check.sh
```

---

## Automate with Cron

Edit cron jobs:

```bash
crontab -e
```

Add the following line:

```bash
0 * * * * /home/vagrant/health_check.sh >> /home/vagrant/health_check.log 2>&1
```

This schedules the script to run every hour and save output to a log file.

---

## Email Notification Feature

Install mail utilities:

```bash
sudo apt install mailutils -y
```

Install Postfix:

```bash
sudo apt install postfix -y
```

During setup, select:

* Internet Site
* Set hostname as default

---

## Email Alert Script (CPU Monitoring)

```bash
#!/bin/bash

CPU_USAGE=$(top -bn1 | grep load | awk '{print $10}' | sed 's/,//')

if (( $(echo "$CPU_USAGE > 0.90" | bc -l) ))
then
    echo "High CPU Usage Detected: $CPU_USAGE" | mail -s "CPU ALERT" your-email@gmail.com
fi
```

---

## System Verification Commands

Check cron jobs:

```bash
crontab -l
```

Check mail queue:

```bash
mailq
```

Check mail logs:

```bash
sudo tail -20 /var/log/mail.log
```

<img width="2560" height="1440" alt="image" src="https://github.com/user-attachments/assets/9fbbece3-6cd0-4e07-895b-90615275903f" />

<img width="2560" height="1440" alt="Screenshot 2026-06-03 161506" src="https://github.com/user-attachments/assets/25d26d00-6c5a-480e-9669-4f3bf1e2a0cf" />

<img width="2560" height="1440" alt="image" src="https://github.com/user-attachments/assets/0945c8e6-b162-49f7-9335-48f4f780deea" />

<img width="2560" height="1440" alt="Screenshot 2026-06-03 161606" src="https://github.com/user-attachments/assets/21b8fd75-1cb1-4b7b-a59a-3259747a6180" />

<img width="2560" height="1440" alt="image" src="https://github.com/user-attachments/assets/e65975d5-09f9-4e3b-9c46-29f0c4375fa6" />

<img width="2560" height="1440" alt="image" src="https://github.com/user-attachments/assets/51ac392d-b97f-46fc-9aa9-c68b876a2868" />

<img width="2560" height="1440" alt="image" src="https://github.com/user-attachments/assets/f25ff327-7bf6-428f-a319-027e0a95d025" />

<img width="2560" height="1440" alt="image" src="https://github.com/user-attachments/assets/4278d363-32d6-48b4-bd5b-af577d9f773e" />

---

## Known Limitation

Email delivery may fail in virtual environments because Postfix is not configured with an external SMTP relay. However, mail generation and alert logic are successfully verified within the system.

---

## Key Learning Outcomes

* Linux shell scripting
* System monitoring automation
* Cron job scheduling
* Process and resource monitoring
* Email alert integration
* System troubleshooting

---

## Environment

* Ubuntu (Vagrant VM)
* Bash Shell
* Postfix
* Mailutils

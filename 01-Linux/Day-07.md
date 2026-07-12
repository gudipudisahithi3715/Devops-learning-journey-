# 🚀 Production Script 1 - Server Health Check Script

# 📖 What is a Server Health Check Script?

A Server Health Check Script is an automation script that checks the overall health of a Linux server.

Instead of manually logging in and checking CPU, memory, disk space, uptime, and running processes, a DevOps Engineer can execute one script that displays all the important information.

It is one of the most commonly used scripts in production environments.

---

# 🎯 Why Do DevOps Engineers Use It?

A DevOps Engineer is often responsible for managing multiple servers.

Checking each server manually is time-consuming.

A health check script provides a quick summary of the server's condition and helps identify issues before they affect applications.

Typical checks include:

- Server hostname
- Current date and time
- Uptime
- Logged-in users
- CPU load
- Memory usage
- Disk usage
- Running processes
- Network information

---

# 📝 Complete Script

```bash
#!/bin/bash

echo "========================================="
echo "        SERVER HEALTH REPORT"
echo "========================================="

echo

echo "Hostname : $(hostname)"

echo "Current Date : $(date)"

echo

echo "---------- Uptime ----------"

uptime

echo

echo "---------- Memory Usage ----------"

free -h

echo

echo "---------- Disk Usage ----------"

df -h

echo

echo "---------- CPU Load ----------"

uptime | awk -F'load average:' '{print $2}'

echo

echo "---------- Logged In Users ----------"

who

echo

echo "---------- Top 5 Processes ----------"

ps -eo pid,comm,%cpu,%mem --sort=-%cpu | head -6

echo

echo "========================================="
echo "Server Health Check Completed"
echo "========================================="
```

---

# 🔍 Line-by-Line Explanation

## Shebang

```bash
#!/bin/bash
```

Specifies that the script should be executed using the Bash shell.

---

## Heading

```bash
echo "SERVER HEALTH REPORT"
```

Prints a readable title.

---

## Hostname

```bash
hostname
```

Displays the server's hostname.

Example:

```
prod-server-01
```

---

## Date

```bash
date
```

Shows the current system date and time.

Useful for knowing exactly when the report was generated.

---

## Uptime

```bash
uptime
```

Shows:

- Current time
- Server uptime
- Number of logged-in users
- Load average

Example

```
14:10:12 up 35 days,
3 users,
load average: 0.15, 0.20, 0.18
```

---

## Memory Usage

```bash
free -h
```

Displays RAM usage in a human-readable format.

Example

```
Total

Used

Free

Buff/Cache

Available
```

This helps determine whether the server is running low on memory.

---

## Disk Usage

```bash
df -h
```

Shows:

- Filesystem
- Total space
- Used space
- Free space
- Usage percentage

A DevOps Engineer checks whether any filesystem is approaching 100% usage.

---

## CPU Load

```bash
uptime | awk -F'load average:' '{print $2}'
```

Extracts only the load average values.

Example

```
0.20, 0.18, 0.16
```

These represent the average CPU load over:

- Last 1 minute
- Last 5 minutes
- Last 15 minutes

---

## Logged-in Users

```bash
who
```

Displays users currently logged into the server.

Useful for identifying active sessions.

---

## Top Processes

```bash
ps -eo pid,comm,%cpu,%mem --sort=-%cpu | head -6
```

Displays:

- PID
- Process name
- CPU usage
- Memory usage

Sorted by highest CPU usage.

The first five processes consuming the most CPU are shown.

---

# 💼 Real-Time DevOps Use Cases

## 1. Morning Server Health Check

Every morning, a DevOps Engineer runs the script to verify that all production servers are healthy before business hours.

---

## 2. Before Deployment

Before deploying a new application version, the engineer checks:

- Available memory
- Disk space
- CPU load

This helps ensure the server has enough resources for the deployment.

---

## 3. Troubleshooting High CPU Usage

If users report slow application performance, the engineer runs the script to identify processes consuming excessive CPU.

---

## 4. Capacity Planning

By monitoring memory and disk usage regularly, the team can determine when servers need upgrades or cleanup.

---

# ⭐ Sample Output

```
=========================================
SERVER HEALTH REPORT
=========================================

Hostname : prod-server

Current Date :
Mon Jul 14 10:00:21 IST 2025

---------- Uptime ----------

10:00:21 up 45 days,
2 users,
load average: 0.21, 0.18, 0.15

---------- Memory Usage ----------

Total: 8GB
Used: 3GB
Free: 2GB
Buff/Cache: 3GB

---------- Disk Usage ----------

Filesystem      Size Used Avail Use%

/dev/sda1        40G 18G 20G 48%

---------- CPU Load ----------

0.21,0.18,0.15

---------- Logged In Users ----------

ubuntu
devops

---------- Top Processes ----------

PID COMMAND %CPU %MEM

2543 java 20.4 15.3

1632 docker 8.3 4.2

221 nginx 1.1 0.5

=========================================
Server Health Check Completed
=========================================
```

---

# ⭐ Best Practices

- Schedule the script using Cron.
- Save the output to a log file.
- Add email notifications if thresholds are exceeded.
- Run the script before deployments.
- Store reports for auditing and troubleshooting.

---

# 🎯 Interview Questions

## 1. What is a Server Health Check Script?

A Server Health Check Script is a shell script that collects important system information such as CPU, memory, disk usage, uptime, and running processes to determine the overall health of a server.

---

## 2. Why is it important in DevOps?

It helps engineers monitor server health, detect issues early, and ensure servers are ready for deployments.

---

## 3. Which commands are commonly used in a health check script?

- `hostname`
- `date`
- `uptime`
- `free -h`
- `df -h`
- `who`
- `ps`
- `top`

---

## 4. How do you check memory usage?

```bash
free -h
```

---

## 5. How do you check disk usage?

```bash
df -h
```

---

## 6. How do you check CPU load?

```bash
uptime
```

or

```bash
top
```

---

## 7. How do you display running processes?

```bash
ps -ef
```

or

```bash
ps aux
```

---

## 8. How can this script be automated?

By scheduling it using Cron (`crontab`) to run at regular intervals, such as every hour or every day.

# 🚀 Production Script 2 - Disk Usage Monitoring Script

# 📖 What is a Disk Usage Monitoring Script?

A Disk Usage Monitoring Script checks the disk utilization of one or more file systems. If disk usage exceeds a predefined threshold, it alerts the administrator so that corrective action can be taken before the disk becomes full.

This helps prevent application failures caused by insufficient disk space.

---

# 🎯 Why Do DevOps Engineers Use It?

In production, servers continuously generate:

- Application logs
- Docker images
- Containers
- Temporary files
- Backup files

Over time, these consume disk space. If a filesystem reaches **100% usage**, applications may fail to write data, logs may stop updating, and services can become unavailable.

To avoid this, DevOps Engineers automate disk monitoring.

---

# 📝 Complete Script

```bash
#!/bin/bash

THRESHOLD=80

echo "Checking Disk Usage..."

df -h | awk 'NR>1 {print $5 " " $6}' | while read output
do
    usage=$(echo $output | awk '{print $1}' | cut -d'%' -f1)
    partition=$(echo $output | awk '{print $2}')

    if [ $usage -ge $THRESHOLD ]
    then
        echo "WARNING: Disk usage on $partition is ${usage}%"
    else
        echo "$partition is healthy (${usage}%)"
    fi
done
```

---

# 🔍 Line-by-Line Explanation

## Define Threshold

```bash
THRESHOLD=80
```

If disk usage is **80% or more**, the script generates a warning.

---

## Display Message

```bash
echo "Checking Disk Usage..."
```

Prints a heading before checking filesystems.

---

## Get Disk Information

```bash
df -h
```

Example output:

```
Filesystem      Size Used Avail Use% Mounted on

/dev/sda1        40G 31G  9G   78% /

/dev/sda2       100G 91G  9G   91% /backup
```

---

## Skip Header

```bash
awk 'NR>1'
```

`NR>1` skips the first line (column headings).

---

## Print Required Columns

```bash
print $5 " " $6
```

Displays:

```
78% /

91% /backup
```

---

## Read Each Line

```bash
while read output
```

Processes each filesystem one at a time.

---

## Extract Usage

```bash
usage=$(echo $output | awk '{print $1}' | cut -d'%' -f1)
```

Converts:

```
91%
```

into

```
91
```

by removing the `%` symbol.

---

## Extract Partition

```bash
partition=$(echo $output | awk '{print $2}')
```

Gets:

```
/
```

or

```
/backup
```

---

## Compare Usage

```bash
if [ $usage -ge $THRESHOLD ]
```

Checks if usage is greater than or equal to the threshold.

---

## Warning

```bash
echo "WARNING: Disk usage on $partition is ${usage}%"
```

Example:

```
WARNING: Disk usage on /backup is 91%
```

---

## Healthy Message

```bash
echo "$partition is healthy (${usage}%)"
```

Example:

```
/ is healthy (45%)
```

---

# 💼 Real-Time DevOps Use Cases

## 1. Production Server Monitoring

Runs every hour using Cron.

If disk usage exceeds 80%, the operations team investigates before the disk fills completely.

---

## 2. Docker Host Monitoring

Checks whether Docker images and containers are consuming excessive disk space.

If usage is high, old images can be removed using:

```bash
docker system prune
```

---

## 3. Kubernetes Nodes

Monitors worker nodes to ensure sufficient storage for pods and container images.

---

## 4. Log Monitoring

Detects when log files are consuming too much disk space so they can be archived or rotated.

---

# ⭐ Sample Output

Healthy Server

```
Checking Disk Usage...

/ is healthy (42%)

/boot is healthy (31%)
```

---

Warning

```
Checking Disk Usage...

WARNING: Disk usage on /backup is 92%

/ is healthy (40%)
```

---

# ⭐ How to Send Email Alerts (Production)

In production, instead of only displaying a warning, administrators often send an email.

Example:

```bash
echo "Disk usage exceeded on $partition" | mail -s "Disk Alert" admin@example.com
```

This automatically notifies the operations team.

---

# ⭐ Schedule Using Cron

Run every 30 minutes:

```bash
*/30 * * * * /home/ubuntu/disk_monitor.sh
```

---

# ⭐ Best Practices

- Monitor all important partitions.
- Set thresholds based on requirements (70%, 80%, or 90%).
- Schedule using Cron.
- Send alerts via email or monitoring tools.
- Rotate logs regularly.
- Clean temporary files periodically.

---

# 🎯 Interview Questions

## 1. Why do we monitor disk usage?

To prevent servers from running out of storage, which can cause applications to fail, logs to stop writing, and services to become unavailable.

---

## 2. Which command checks disk usage?

```bash
df -h
```

---

## 3. What does `-h` mean?

It displays sizes in a human-readable format (KB, MB, GB).

---

## 4. What is the purpose of `cut -d'%' -f1`?

It removes the `%` symbol so the value can be compared numerically.

Example:

```
85%
```

becomes

```
85
```

---

## 5. Why do we use a threshold?

To generate alerts before the disk becomes completely full, allowing preventive action.

---

## 6. How is this script automated?

Using Cron:

```bash
crontab -e
```

Example:

```bash
*/30 * * * * /home/ubuntu/disk_monitor.sh
```

---

## 7. How would you improve this script for production?

- Send email or Slack alerts.
- Write warnings to a log file.
- Ignore temporary filesystems.
- Monitor multiple servers.
- Integrate with monitoring tools like Prometheus or Nagios.

---

# ⭐ Commands Used

| Command | Purpose |
|---------|---------|
| `df -h` | Show disk usage |
| `awk` | Extract columns |
| `cut` | Remove `%` symbol |
| `while read` | Process each filesystem |
| `if` | Compare threshold |
| `echo` | Print output |

---

# ✅ Script #2 Completed

Topics Revised:

✔ Variables

✔ if Statement

✔ while Loop

✔ awk

✔ cut

✔ df

✔ Cron Jobs

✔ Disk Monitoring

✔ Production Automation
# 🚀 Production Script 3 - Memory Usage Monitoring Script

# 📖 What is a Memory Usage Monitoring Script?

A Memory Usage Monitoring Script checks the RAM utilization of a Linux server. If memory usage exceeds a predefined threshold, it generates an alert so that the DevOps team can take corrective action.

This helps prevent application slowdowns and Out of Memory (OOM) errors.

---

# 🎯 Why Do DevOps Engineers Use It?

Applications like:

- Jenkins
- SonarQube
- Java Applications
- Kubernetes
- Databases

consume RAM continuously.

If available memory becomes too low:

- Applications become slow
- New processes cannot start
- Linux may kill processes automatically (OOM Killer)
- Production outages may occur

To avoid these issues, memory monitoring is automated.

---

# 📝 Complete Script

```bash
#!/bin/bash

THRESHOLD=80

echo "Checking Memory Usage..."

memory_usage=$(free | awk '/Mem:/ {printf("%.0f"), $3/$2 * 100}')

echo "Current Memory Usage: ${memory_usage}%"

if [ $memory_usage -ge $THRESHOLD ]
then
    echo "WARNING: Memory usage is above ${THRESHOLD}%"
else
    echo "Memory usage is normal."
fi
```

---

# 🔍 Line-by-Line Explanation

## Set Threshold

```bash
THRESHOLD=80
```

If memory usage reaches **80% or more**, a warning is displayed.

---

## Display Heading

```bash
echo "Checking Memory Usage..."
```

Prints a heading.

---

## Get Memory Usage

```bash
free
```

Example output:

```
              total   used   free

Mem:          7986   4020   1800

Swap:         2047      0   2047
```

---

## Calculate Percentage

```bash
memory_usage=$(free | awk '/Mem:/ {printf("%.0f"), $3/$2 * 100}')
```

Explanation:

```
$2 → Total Memory

$3 → Used Memory
```

Formula:

```
(Used Memory / Total Memory) × 100
```

Example

```
4000 / 8000 × 100 = 50%
```

The result is stored in:

```
memory_usage
```

---

## Display Current Usage

```bash
echo "Current Memory Usage: ${memory_usage}%"
```

Example

```
Current Memory Usage: 52%
```

---

## Compare with Threshold

```bash
if [ $memory_usage -ge $THRESHOLD ]
```

Checks whether memory usage is greater than or equal to 80%.

---

## Warning

```bash
echo "WARNING: Memory usage is above ${THRESHOLD}%"
```

Example

```
WARNING: Memory usage is above 80%
```

---

## Healthy Message

```bash
echo "Memory usage is normal."
```

---

# ⭐ Sample Output

Healthy

```
Checking Memory Usage...

Current Memory Usage: 48%

Memory usage is normal.
```

---

High Memory

```
Checking Memory Usage...

Current Memory Usage: 91%

WARNING: Memory usage is above 80%
```

---

# 💼 Real-Time DevOps Use Cases

## 1. Jenkins Server

If Jenkins memory usage becomes very high, builds may become slow or fail.

This script alerts the DevOps team before Jenkins becomes unstable.

---

## 2. Kubernetes Worker Node

If a node is running out of RAM, pods may be evicted or restarted.

Memory monitoring helps prevent these issues.

---

## 3. Java Applications

Java applications (Spring Boot, Tomcat, SonarQube) consume significant memory.

Monitoring helps identify memory leaks before they affect production.

---

## 4. Database Servers

Databases such as MySQL and PostgreSQL use RAM for caching.

This script helps ensure enough memory remains available.

---

# ⭐ Production Improvements

Instead of only displaying a warning, production scripts usually:

- Send email alerts
- Send Slack or Microsoft Teams notifications
- Write warnings to log files
- Trigger monitoring systems (Prometheus, Nagios, Zabbix)

Example:

```bash
echo "Memory usage exceeded 80%" | mail -s "Memory Alert" admin@example.com
```

---

# ⭐ Schedule Using Cron

Run every 10 minutes:

```bash
*/10 * * * * /home/ubuntu/memory_monitor.sh
```

---

# ⭐ Best Practices

- Monitor both RAM and Swap.
- Set appropriate thresholds (70%, 80%, or 90%).
- Record memory usage trends.
- Combine with CPU and disk monitoring.
- Investigate sustained high memory usage instead of occasional spikes.

---

# 🎯 Interview Questions

## 1. Why do we monitor memory usage?

To prevent applications from slowing down, crashing, or being terminated due to insufficient memory.

---

## 2. Which command checks memory usage?

```bash
free -h
```

or

```bash
free
```

---

## 3. What does `free -h` show?

It displays:

- Total Memory
- Used Memory
- Free Memory
- Shared Memory
- Buffers/Cache
- Available Memory

in a human-readable format.

---

## 4. How is memory usage percentage calculated?

```
(Used Memory / Total Memory) × 100
```

---

## 5. Why do we use a threshold?

To detect high memory usage before it impacts applications or causes system instability.

---

## 6. How can this script be automated?

Using Cron.

Example:

```bash
*/10 * * * * /home/ubuntu/memory_monitor.sh
```

---

## 7. How would you improve this script for production?

- Send email or Slack alerts.
- Log memory usage history.
- Monitor swap usage.
- Integrate with Prometheus or Grafana.
- Generate alerts only after repeated threshold breaches to reduce false alarms.

---

# ⭐ Commands Used

| Command | Purpose |
|---------|---------|
| `free` | Display memory usage |
| `free -h` | Human-readable memory usage |
| `awk` | Calculate percentage |
| `if` | Compare threshold |
| `echo` | Display messages |
| `cron` | Schedule execution |

---

# ✅ Script #3 Completed

Topics Revised:

✔ Variables

✔ Command Substitution (`$(...)`)

✔ `free`

✔ `awk`

✔ `if`

✔ Threshold Monitoring

✔ Cron Jobs

✔ Memory Monitoring

✔ Production Automation

# 🚀 Production Script 4 - CPU Usage Monitoring Script

# 📖 What is a CPU Usage Monitoring Script?

A CPU Usage Monitoring Script checks how much CPU is being utilized by the system. If CPU usage crosses a predefined threshold, the script alerts the administrator.

CPU monitoring helps identify performance bottlenecks before they affect applications.

---

# 🎯 Why Do DevOps Engineers Monitor CPU?

Applications running on servers consume CPU resources.

Examples:

- Jenkins builds
- Docker containers
- Kubernetes Pods
- Java applications
- Databases
- Nginx
- Apache

If CPU usage remains high:

- Applications become slow.
- API response times increase.
- Builds take longer.
- Containers may become unresponsive.
- Users experience poor performance.

---

# 📝 Complete Script

```bash
#!/bin/bash

THRESHOLD=80

echo "Checking CPU Usage..."

cpu_usage=$(top -bn1 | grep "Cpu(s)" | awk '{print int($2+$4)}')

echo "Current CPU Usage: ${cpu_usage}%"

if [ $cpu_usage -ge $THRESHOLD ]
then
    echo "WARNING: CPU usage is above ${THRESHOLD}%"
else
    echo "CPU usage is normal."
fi
```

---

# 🔍 Line-by-Line Explanation

## Define Threshold

```bash
THRESHOLD=80
```

If CPU usage exceeds **80%**, the script displays a warning.

---

## Display Heading

```bash
echo "Checking CPU Usage..."
```

---

## Get CPU Usage

```bash
top -bn1
```

### What do these options mean?

```
-b
```

Batch mode (non-interactive output)

```
-n1
```

Run only once.

This makes `top` suitable for shell scripts.

---

Example output

```
Cpu(s): 15.4 us, 5.0 sy, 0.0 ni,
79.6 id,
0.0 wa,
0.0 hi,
0.0 si
```

Where:

```
us = User CPU

sy = System CPU

id = Idle CPU
```

---

## Extract CPU Usage

```bash
awk '{print int($2+$4)}'
```

It adds:

```
User CPU

+

System CPU
```

Example

```
15 + 5 = 20%
```

The value is stored in:

```bash
cpu_usage
```

---

## Display CPU Usage

```bash
echo "Current CPU Usage: ${cpu_usage}%"
```

Example

```
Current CPU Usage: 23%
```

---

## Compare Threshold

```bash
if [ $cpu_usage -ge $THRESHOLD ]
```

If CPU usage is greater than or equal to 80%, display a warning.

---

## Warning

```bash
echo "WARNING: CPU usage is above 80%"
```

---

## Healthy Message

```bash
echo "CPU usage is normal."
```

---

# ⭐ Sample Output

Healthy

```
Checking CPU Usage...

Current CPU Usage: 21%

CPU usage is normal.
```

---

High CPU

```
Checking CPU Usage...

Current CPU Usage: 91%

WARNING: CPU usage is above 80%
```

---

# ⭐ Identify Top CPU-Consuming Processes

When CPU usage is high, we need to know **which process is responsible**.

Command:

```bash
ps -eo pid,comm,%cpu --sort=-%cpu | head
```

Example

```
PID COMMAND %CPU

2351 java 65

1732 docker 15

211 nginx 3
```

This helps identify the application consuming the most CPU.

---

# 💼 Real-Time DevOps Use Cases

## 1. Jenkins Server

Multiple builds running simultaneously can consume excessive CPU.

The script alerts the DevOps team before Jenkins performance degrades.

---

## 2. Kubernetes Worker Node

If CPU usage stays high, pods may become slow or be throttled.

Monitoring helps decide whether to scale workloads or add more nodes.

---

## 3. Docker Host

A container consuming too much CPU can affect other containers running on the same host.

---

## 4. Java Applications

High CPU usage may indicate:

- Infinite loops
- Heavy garbage collection
- Poorly optimized code
- Excessive requests

---

# ⭐ Production Improvements

Instead of simply displaying warnings:

- Send email alerts.
- Send Slack notifications.
- Log CPU history.
- Integrate with Prometheus.
- Generate alerts after repeated threshold violations.

Example

```bash
echo "High CPU Usage" | mail -s "CPU Alert" admin@example.com
```

---

# ⭐ Schedule Using Cron

Run every 5 minutes

```bash
*/5 * * * * /home/ubuntu/cpu_monitor.sh
```

---

# ⭐ Best Practices

- Monitor CPU together with memory and disk.
- Investigate sustained high CPU usage.
- Identify the top CPU-consuming processes.
- Monitor historical CPU trends.
- Avoid using a single threshold for every workload.

---

# 🎯 Interview Questions

## 1. Why do we monitor CPU usage?

To detect performance issues before they impact applications or users.

---

## 2. Which command displays CPU usage?

```bash
top
```

or

```bash
top -bn1
```

---

## 3. Why is `top -bn1` used in scripts?

Because it runs in batch mode and exits after one iteration, making it suitable for automation.

---

## 4. How do you identify the process consuming the most CPU?

```bash
ps -eo pid,comm,%cpu --sort=-%cpu | head
```

---

## 5. How do you automate CPU monitoring?

Using Cron.

Example

```bash
*/5 * * * * /home/ubuntu/cpu_monitor.sh
```

---

## 6. What would you do if CPU usage is constantly above 90%?

Possible steps:

- Identify the high CPU process.
- Check application logs.
- Restart the affected service if appropriate.
- Scale the application horizontally or vertically.
- Optimize application code or configuration.
- Increase server resources if necessary.

---

## 7. Which Linux commands are commonly used to monitor CPU?

- `top`
- `htop`
- `uptime`
- `ps`
- `vmstat`
- `sar`

---

# ⭐ Commands Used

| Command | Purpose |
|----------|----------|
| `top -bn1` | Get CPU usage |
| `grep` | Filter output |
| `awk` | Extract values |
| `ps` | List processes |
| `head` | Show top entries |
| `cron` | Schedule execution |

---

# ✅ Script #4 Completed

Topics Revised

✔ Variables

✔ Command Substitution

✔ top

✔ grep

✔ awk

✔ ps

✔ head

✔ if Statement

✔ Cron Jobs

✔ CPU Monitoring

✔ Production Automation

# 🚀 Production Script 5 - Service Monitoring & Auto Restart Script

# 📖 What is a Service Monitoring Script?

A Service Monitoring Script checks whether a Linux service is running.

If the service is stopped, the script automatically starts (or restarts) it and logs the action.

This improves application availability and reduces downtime.

---

# 🎯 Why Do DevOps Engineers Use It?

Production servers run many critical services:

- Docker
- Jenkins
- Nginx
- Apache
- MySQL
- PostgreSQL
- Tomcat
- SSH

If any of these services stop unexpectedly:

- Applications become unavailable.
- CI/CD pipelines fail.
- Websites go offline.
- Users experience downtime.

Instead of manually checking services, DevOps Engineers automate this process.

---

# 📝 Complete Script

```bash
#!/bin/bash

SERVICE="docker"

echo "Checking $SERVICE service..."

if systemctl is-active --quiet $SERVICE
then
    echo "$SERVICE service is running."
else
    echo "$SERVICE service is stopped."
    echo "Restarting $SERVICE service..."

    sudo systemctl start $SERVICE

    echo "$SERVICE service started successfully."
fi
```

---

# 🔍 Line-by-Line Explanation

## Store Service Name

```bash
SERVICE="docker"
```

Instead of hardcoding the service name multiple times, we store it in a variable.

You can replace:

```
docker
```

with

```
jenkins
nginx
apache2
tomcat
mysql
```

---

## Display Message

```bash
echo "Checking $SERVICE service..."
```

Example

```
Checking docker service...
```

---

## Check Service Status

```bash
systemctl is-active --quiet docker
```

### What does it do?

Checks whether the service is currently active.

The `--quiet` option suppresses output and returns only an exit status.

- Exit status `0` → Service is running
- Non-zero → Service is stopped

---

## Service Running

```bash
echo "$SERVICE service is running."
```

Example

```
docker service is running.
```

---

## Service Stopped

```bash
echo "$SERVICE service is stopped."
```

---

## Start Service

```bash
sudo systemctl start docker
```

Starts the service if it is stopped.

---

## Confirmation

```bash
echo "$SERVICE service started successfully."
```

---

# ⭐ Sample Output

## Service Running

```
Checking docker service...

docker service is running.
```

---

## Service Stopped

```
Checking docker service...

docker service is stopped.

Restarting docker service...

docker service started successfully.
```

---

# ⭐ Improved Version

After restarting, verify that the service actually started.

```bash
#!/bin/bash

SERVICE="docker"

if systemctl is-active --quiet $SERVICE
then
    echo "$SERVICE is already running."

else

    echo "$SERVICE stopped."

    sudo systemctl start $SERVICE

    if systemctl is-active --quiet $SERVICE
    then
        echo "$SERVICE restarted successfully."
    else
        echo "Failed to restart $SERVICE."
    fi

fi
```

This is closer to what you would use in production.

---

# ⭐ Monitoring Multiple Services

```bash
#!/bin/bash

services=("docker" "nginx" "jenkins")

for service in "${services[@]}"
do

    if systemctl is-active --quiet $service
    then
        echo "$service is running."

    else

        echo "$service stopped."

        sudo systemctl start $service

        echo "$service restarted."

    fi

done
```

This script checks all listed services one by one.

---

# 💼 Real-Time DevOps Use Cases

## 1. Docker Host

Automatically restart Docker if it stops.

---

## 2. Jenkins Server

Restart Jenkins if it crashes due to memory issues.

---

## 3. Nginx Load Balancer

Restart Nginx if the service stops responding.

---

## 4. Database Server

Restart MySQL or PostgreSQL when the service unexpectedly stops.

---

## 5. Kubernetes Control Plane

Monitor critical services running on control-plane nodes.

---

# ⭐ Automate Using Cron

Run every minute.

```bash
* * * * * /home/ubuntu/service_monitor.sh
```

This checks the service every minute.

---

# ⭐ Save Logs

Instead of only displaying messages:

```bash
echo "$(date): Docker restarted." >> service.log
```

Example

```
Mon Jul 14 10:00:12

Docker restarted.
```

Keeping logs helps during troubleshooting and audits.

---

# ⭐ Best Practices

- Check service status before restarting.
- Verify the restart was successful.
- Log all restart events.
- Send alerts for repeated failures.
- Avoid restarting services continuously if they keep failing.

---

# 🎯 Interview Questions

## 1. Why do we monitor services?

To ensure critical applications remain available and recover automatically if a service stops.

---

## 2. Which command checks whether a service is running?

```bash
systemctl is-active service_name
```

Example

```bash
systemctl is-active docker
```

---

## 3. Which command starts a service?

```bash
sudo systemctl start docker
```

---

## 4. Which command restarts a service?

```bash
sudo systemctl restart docker
```

---

## 5. Which command checks service status?

```bash
systemctl status docker
```

---

## 6. How do you automate service monitoring?

Using Cron.

Example

```bash
* * * * * /home/ubuntu/service_monitor.sh
```

---

## 7. How would you improve this script for production?

Possible improvements:

- Write restart events to a log file.
- Send email or Slack alerts.
- Verify the restart succeeded.
- Monitor multiple services.
- Integrate with Prometheus, Grafana, or Nagios.
- Add retry logic before alerting administrators.

---

# ⭐ Commands Used

| Command | Purpose |
|----------|----------|
| `systemctl is-active` | Check if a service is running |
| `systemctl start` | Start a service |
| `systemctl restart` | Restart a service |
| `systemctl status` | View service status |
| `echo` | Display or log messages |
| `cron` | Schedule automatic checks |

---

# ✅ Script #5 Completed

Topics Revised

✔ Variables

✔ if-else

✔ systemctl

✔ Arrays

✔ for Loop

✔ Cron Jobs

✔ Logging

✔ Service Monitoring

✔ Production Automation

# 🚀 Production Script 6 - Backup Automation Script

# 📖 What is a Backup Automation Script?

A Backup Automation Script automatically creates a backup of important files or directories and stores them in a backup location.

Instead of manually copying files every day, the script performs the backup automatically.

Backups are one of the most critical responsibilities of a DevOps Engineer because they help recover data in case of accidental deletion, server failure, or application issues.

---

# 🎯 Why Do DevOps Engineers Use It?

Production servers contain:

- Application source code
- Configuration files
- Log files
- Databases
- SSL certificates
- Jenkins configurations

If any of these files are lost, it can lead to downtime or data loss.

Regular automated backups ensure that important data can be restored quickly.

---

# 📝 Complete Script

```bash
#!/bin/bash

SOURCE="/home/ubuntu/project"

DESTINATION="/home/ubuntu/backups"

DATE=$(date +%Y-%m-%d_%H-%M-%S)

BACKUP_FILE="backup_$DATE.tar.gz"

echo "Starting Backup..."

tar -czf $DESTINATION/$BACKUP_FILE $SOURCE

echo "Backup Completed Successfully."

echo "Backup File: $DESTINATION/$BACKUP_FILE"
```

---

# 🔍 Line-by-Line Explanation

## Source Directory

```bash
SOURCE="/home/ubuntu/project"
```

Directory that needs to be backed up.

---

## Backup Location

```bash
DESTINATION="/home/ubuntu/backups"
```

Location where backup files will be stored.

---

## Current Date

```bash
DATE=$(date +%Y-%m-%d_%H-%M-%S)
```

Example

```
2026-07-13_09-30-15
```

Adding the timestamp prevents backup files from overwriting each other.

---

## Backup File Name

```bash
BACKUP_FILE="backup_$DATE.tar.gz"
```

Example

```
backup_2026-07-13_09-30-15.tar.gz
```

---

## Create Backup

```bash
tar -czf $DESTINATION/$BACKUP_FILE $SOURCE
```

### Meaning of options

```
-c
```

Create archive

```
-z
```

Compress using gzip

```
-f
```

Specify filename

---

## Display Success Message

```bash
echo "Backup Completed Successfully."
```

---

# ⭐ Sample Output

```
Starting Backup...

Backup Completed Successfully.

Backup File:
/home/ubuntu/backups/backup_2026-07-13_09-30-15.tar.gz
```

---

# ⭐ Verify Backup

List backup files:

```bash
ls -lh /home/ubuntu/backups
```

Example

```
backup_2026-07-13_09-30-15.tar.gz
```

---

# ⭐ Restore Backup

Extract backup:

```bash
tar -xzf backup_2026-07-13_09-30-15.tar.gz
```

Options:

```
-x
```

Extract

```
-z
```

Extract gzip archive

```
-f
```

Archive file

---

# ⭐ Production Version

Before taking the backup, ensure the destination directory exists.

```bash
if [ ! -d "$DESTINATION" ]
then
    mkdir -p "$DESTINATION"
fi
```

This creates the backup directory automatically if it does not exist.

---

# ⭐ Delete Old Backups

Keep only backups newer than 7 days.

```bash
find $DESTINATION -name "*.tar.gz" -mtime +7 -delete
```

Meaning:

```
-mtime +7
```

Files older than 7 days.

```
-delete
```

Delete matching files.

---

# ⭐ Schedule Using Cron

Take a backup every day at 2:00 AM.

```bash
0 2 * * * /home/ubuntu/backup.sh
```

---

# 💼 Real-Time DevOps Use Cases

## 1. Jenkins Backup

Backup:

- Jenkins jobs
- Plugins
- Configuration files

---

## 2. Application Backup

Backup application source code before deployment.

---

## 3. Configuration Backup

Backup:

- Nginx configuration
- Apache configuration
- Docker Compose files
- Kubernetes YAML files

---

## 4. Database Backup

Store database dumps daily before maintenance.

---

## 5. Log Backup

Archive important logs before log rotation.

---

# ⭐ Best Practices

- Compress backups to save space.
- Include timestamps in filenames.
- Verify backup completion.
- Store backups on a different server or cloud storage.
- Test backup restoration regularly.
- Remove old backups automatically.

---

# 🎯 Interview Questions

## 1. Why are backups important?

Backups protect important data and allow systems to be restored after accidental deletion, hardware failure, or application issues.

---

## 2. Which command is commonly used to create compressed backups?

```bash
tar -czf
```

---

## 3. What does `tar -czf` mean?

- `-c` → Create archive
- `-z` → Compress using gzip
- `-f` → Specify output filename

---

## 4. How do you restore a backup?

```bash
tar -xzf backup.tar.gz
```

---

## 5. Why do we include the date in backup filenames?

To create unique backup files and avoid overwriting previous backups.

---

## 6. How do you schedule automatic backups?

Using Cron.

Example:

```bash
0 2 * * * /home/ubuntu/backup.sh
```

---

## 7. How would you improve this script for production?

- Verify backup success.
- Log backup operations.
- Upload backups to AWS S3 or Azure Blob Storage.
- Encrypt sensitive backups.
- Send email or Slack notifications.
- Delete old backups automatically.

---

# ⭐ Commands Used

| Command | Purpose |
|----------|----------|
| `tar -czf` | Create compressed archive |
| `tar -xzf` | Extract archive |
| `date` | Generate timestamp |
| `mkdir -p` | Create directory if it doesn't exist |
| `find` | Find files |
| `-mtime` | Filter by file age |
| `-delete` | Delete old files |
| `cron` | Schedule backups |

---

# ✅ Script #6 Completed

Topics Revised

✔ Variables

✔ Command Substitution

✔ tar

✔ date

✔ if Statement

✔ mkdir

✔ find

✔ Cron Jobs

✔ Backup Automation

✔ Production Best Practices

# 🚀 Production Script 7 - Log Cleanup Script

# 📖 What is a Log Cleanup Script?

A Log Cleanup Script automatically removes old log files from a server.

Applications continuously generate log files. If these logs are never cleaned up, they consume disk space and can eventually fill the filesystem, causing applications to fail.

This script helps free up disk space by deleting logs older than a specified number of days.

---

# 🎯 Why Do DevOps Engineers Use It?

Almost every application generates logs:

- Jenkins
- Docker
- Nginx
- Apache
- Tomcat
- Kubernetes
- Java Applications
- Linux System Logs

If these logs are not managed properly:

- Disk space becomes full.
- Applications cannot write new logs.
- Deployments may fail.
- Server performance may degrade.

To prevent these issues, log cleanup is automated.

---

# 📝 Complete Script

```bash
#!/bin/bash

LOG_DIR="/var/log/myapp"

DAYS=7

echo "Cleaning log files older than $DAYS days..."

find $LOG_DIR -type f -name "*.log" -mtime +$DAYS -delete

echo "Log cleanup completed."
```

---

# 🔍 Line-by-Line Explanation

## Log Directory

```bash
LOG_DIR="/var/log/myapp"
```

Specifies the directory containing log files.

Replace this with your application's log directory.

Example:

```
/var/log/nginx

/var/log/apache2

/var/log/jenkins

/var/log
```

---

## Number of Days

```bash
DAYS=7
```

Delete log files older than **7 days**.

You can change it to:

```
15

30

90
```

depending on your organization's log retention policy.

---

## Display Message

```bash
echo "Cleaning log files older than $DAYS days..."
```

Displays a message before cleanup starts.

---

## Find Old Log Files

```bash
find $LOG_DIR
```

Searches inside the specified directory.

---

## Search Only Files

```bash
-type f
```

Ensures only regular files are selected.

Directories are ignored.

---

## Match Log Files

```bash
-name "*.log"
```

Only files ending with:

```
.log
```

are selected.

Example:

```
app.log

access.log

error.log
```

---

## Find Files Older Than 7 Days

```bash
-mtime +7
```

Meaning:

```
+7
```

Older than 7 days.

Examples

```
1 day old

❌ Not deleted

8 days old

✅ Deleted
```

---

## Delete Files

```bash
-delete
```

Deletes the matched files.

---

## Completion Message

```bash
echo "Log cleanup completed."
```

Displays a success message after cleanup.

---

# ⭐ Sample Output

```
Cleaning log files older than 7 days...

Log cleanup completed.
```

---

# ⭐ Verify Before Deleting (Recommended)

Instead of deleting immediately, first check which files will be deleted.

```bash
find $LOG_DIR -type f -name "*.log" -mtime +7
```

Example

```
error.log

app.log

server.log
```

After verifying, add:

```bash
-delete
```

---

# ⭐ Archive Logs Before Deleting (Production)

Instead of deleting immediately, compress old logs.

```bash
tar -czf logs_backup.tar.gz *.log
```

Then delete the originals.

This allows recovery if logs are needed later.

---

# ⭐ Production Version

```bash
#!/bin/bash

LOG_DIR="/var/log/myapp"

find $LOG_DIR -type f -name "*.log" -mtime +7 \
-exec gzip {} \;
```

Instead of deleting, it compresses old log files to save disk space.

---

# ⭐ Schedule Using Cron

Run every night at 1:00 AM.

```bash
0 1 * * * /home/ubuntu/log_cleanup.sh
```

This automatically cleans old logs daily.

---

# 💼 Real-Time DevOps Use Cases

## 1. Nginx Logs

Clean:

```
access.log

error.log
```

to prevent the web server disk from filling.

---

## 2. Jenkins Logs

Remove old build logs to save storage.

---

## 3. Docker Logs

Delete old container logs after they are archived.

---

## 4. Application Logs

Java applications continuously generate logs.

Cleaning old logs prevents disk exhaustion.

---

## 5. Kubernetes Worker Nodes

Remove archived logs from worker nodes to maintain sufficient storage.

---

# ⭐ Best Practices

- Verify files before deleting.
- Compress logs before deletion if required.
- Use logrotate for standard Linux log management.
- Schedule cleanup using Cron.
- Keep logs according to company retention policies.
- Never delete active log files.

---

# 🎯 Interview Questions

## 1. Why is log cleanup important?

It prevents disk space from filling up, ensuring applications can continue writing logs and the server remains healthy.

---

## 2. Which command is commonly used to find old log files?

```bash
find
```

---

## 3. What does this command do?

```bash
find /var/log -type f -name "*.log" -mtime +7
```

It finds all `.log` files older than 7 days in `/var/log`.

---

## 4. What is the purpose of `-mtime +7`?

It selects files modified more than 7 days ago.

---

## 5. Why should you verify files before deleting them?

To avoid accidentally deleting important or active log files.

---

## 6. Which Linux utility is commonly used for automatic log management?

```
logrotate
```

---

## 7. How do you automate log cleanup?

Using Cron.

Example:

```bash
0 1 * * * /home/ubuntu/log_cleanup.sh
```

---

## 8. How would you improve this script for production?

- Archive logs before deletion.
- Log cleanup activity.
- Send alerts if cleanup fails.
- Exclude important log files.
- Compress logs using `gzip`.
- Use `logrotate` where appropriate.

---

# ⭐ Commands Used

| Command | Purpose |
|----------|----------|
| `find` | Search files |
| `-type f` | Select regular files |
| `-name "*.log"` | Match log files |
| `-mtime` | Filter by file age |
| `-delete` | Delete files |
| `gzip` | Compress files |
| `tar` | Archive files |
| `cron` | Schedule cleanup |

---

# ✅ Script #7 Completed

Topics Revised

✔ Variables

✔ find Command

✔ File Filtering

✔ File Deletion

✔ File Compression

✔ tar

✔ gzip

✔ Cron Jobs

✔ Log Management

✔ Production Automation

### Interview Scenario

Interviewer: "One of your production servers shows 100% disk utilization. What will you do?"

Answer:

Check disk usage:

df -h

Find large directories:

du -sh /* | sort -hr

Check log directory:

cd /var/log
ls -lh

Find old log files:

find /var/log -type f -name "*.log" -mtime +7
Archive or delete old logs if they are no longer needed.

Verify that disk space has been freed:

df -h

# 🚀 Production Script 8 - User Creation Automation Script

# 📖 What is a User Creation Automation Script?

A User Creation Automation Script automates the process of creating Linux users.

Instead of manually executing multiple Linux commands, the script creates the user, sets the password, assigns groups, and verifies that the user was created successfully.

This saves time and ensures consistency.

---

# 🎯 Why Do DevOps Engineers Use It?

In production environments, multiple users may need access to servers.

Examples:

- New DevOps Engineers
- Developers
- QA Engineers
- Database Administrators
- Support Engineers

Instead of creating users manually every time, automation simplifies the process.

---

# 📝 Complete Script

```bash
#!/bin/bash

USERNAME=$1

PASSWORD=$2

echo "Creating User..."

sudo useradd -m $USERNAME

echo "$USERNAME:$PASSWORD" | sudo chpasswd

echo "User created successfully."
```

---

# 🔍 Line-by-Line Explanation

## Get Username

```bash
USERNAME=$1
```

Reads the first command-line argument.

Example

```bash
./create_user.sh sahithi Welcome@123
```

Here,

```
$1 = sahithi
```

---

## Get Password

```bash
PASSWORD=$2
```

Reads the second command-line argument.

```
$2 = Welcome@123
```

---

## Display Message

```bash
echo "Creating User..."
```

---

## Create User

```bash
sudo useradd -m $USERNAME
```

### Meaning

```
useradd
```

Creates a new Linux user.

```
-m
```

Creates the user's home directory.

Example

```
/home/sahithi
```

---

## Set Password

```bash
echo "$USERNAME:$PASSWORD" | sudo chpasswd
```

`chpasswd` reads the username and password from standard input and sets the user's password.

---

## Display Success Message

```bash
echo "User created successfully."
```

---

# ▶️ How to Execute

Give execute permission:

```bash
chmod +x create_user.sh
```

Run the script:

```bash
./create_user.sh sahithi Welcome@123
```

---

# ⭐ Verify User

Check if the user exists:

```bash
id sahithi
```

Example output

```
uid=1002(sahithi)

gid=1002(sahithi)

groups=1002(sahithi)
```

---

Check home directory:

```bash
ls /home
```

Example

```
sahithi
```

---

# ⭐ Improved Production Version

Before creating the user, check whether it already exists.

```bash
#!/bin/bash

USERNAME=$1

PASSWORD=$2

if id "$USERNAME" &>/dev/null
then
    echo "User already exists."

else

    sudo useradd -m $USERNAME

    echo "$USERNAME:$PASSWORD" | sudo chpasswd

    echo "User Created Successfully."

fi
```

This prevents duplicate user creation.

---

# ⭐ Add User to a Group

Example:

```bash
sudo usermod -aG docker $USERNAME
```

This adds the user to the Docker group.

Other examples:

```
sudo usermod -aG sudo username

sudo usermod -aG developers username
```

---

# ⭐ Create Multiple Users

```bash
#!/bin/bash

users=("dev1" "dev2" "qa1")

for user in "${users[@]}"
do

    sudo useradd -m $user

    echo "$user:Password@123" | sudo chpasswd

done
```

This creates multiple users automatically.

---

# ⭐ Save Logs

```bash
echo "$(date): User $USERNAME created." >> user_creation.log
```

Example

```
Mon Jul 14 09:00:12

User sahithi created.
```

---

# 💼 Real-Time DevOps Use Cases

## 1. Employee Onboarding

When a new employee joins, create their Linux account automatically.

---

## 2. Training Servers

Create multiple users for students attending DevOps training.

---

## 3. Temporary Project Access

Automatically create temporary accounts for consultants or vendors.

---

## 4. Cloud Virtual Machines

Provision user accounts on newly created EC2 or Azure Virtual Machines.

---

## 5. Automation Tools

Use this script within Ansible or Jenkins jobs to provision user accounts consistently.

---

# ⭐ Schedule Using Cron (Optional)

Although user creation is usually event-driven, a script can process a file of new users periodically.

Example:

```bash
0 9 * * * /home/ubuntu/create_users.sh
```

---

# ⭐ Best Practices

- Validate user input before creating accounts.
- Check if the user already exists.
- Use strong passwords.
- Avoid hardcoding passwords in scripts.
- Add users only to required groups (principle of least privilege).
- Log account creation events.

---

# 🎯 Interview Questions

## 1. Which command creates a Linux user?

```bash
useradd
```

---

## 2. What does `-m` do?

It creates the user's home directory.

Example:

```
/home/username
```

---

## 3. Which command sets a user's password?

```bash
passwd
```

or for automation:

```bash
chpasswd
```

---

## 4. Which command checks whether a user exists?

```bash
id username
```

---

## 5. Which command adds a user to a group?

```bash
usermod -aG groupname username
```

Example:

```bash
sudo usermod -aG docker sahithi
```

---

## 6. Why should scripts check if a user already exists?

To prevent duplicate accounts and avoid errors during automation.

---

## 7. How would you improve this script for production?

- Validate input parameters.
- Check for duplicate users.
- Generate random passwords securely.
- Log all user creation activities.
- Send notification emails.
- Assign appropriate groups and permissions automatically.

---

# ⭐ Commands Used

| Command | Purpose |
|----------|----------|
| `useradd` | Create a new user |
| `useradd -m` | Create user with home directory |
| `chpasswd` | Set password non-interactively |
| `id` | Check if user exists |
| `usermod -aG` | Add user to a group |
| `chmod +x` | Make script executable |
| `echo` | Display messages or write logs |

---

# ✅ Script #8 Completed

Topics Revised

✔ Command-Line Arguments

✔ Variables

✔ useradd

✔ chpasswd

✔ usermod

✔ id

✔ if Statement

✔ Logging

✔ Linux User Management

✔ Production Automation


# Interview Scenario
Interviewer:

"You have joined a company where 50 new developers need Linux accounts by the end of the day. How would you handle it?"

Answer:

Instead of creating users manually:

Prepare a file containing usernames.
Write a shell script that:
Reads usernames from the file.
Checks whether each user already exists.
Creates the user with useradd -m.
Sets a temporary password using chpasswd.
Adds the user to the required groups (e.g., developers or docker).
Logs successful and failed user creations.

Verify the accounts using:

id username

This approach is faster, consistent, and reduces manual errors.

# 🚀 Production Script 9 - Docker Cleanup Script

# 📖 What is a Docker Cleanup Script?

A Docker Cleanup Script removes unused Docker resources to free disk space and keep the Docker host clean.

It can remove:

- Stopped containers
- Dangling images
- Unused images
- Unused networks
- Unused volumes
- Build cache

This script is commonly scheduled using Cron in production environments.

---

# 🎯 Why Do DevOps Engineers Use It?

During CI/CD pipelines:

- Hundreds of Docker images are built.
- Temporary containers are created.
- Old containers stop after deployments.
- Unused networks and volumes remain.

If these resources are not cleaned regularly:

- Disk usage increases.
- Docker builds become slower.
- Deployments may fail due to insufficient storage.

---

# 📝 Complete Script

```bash
#!/bin/bash

echo "Starting Docker Cleanup..."

docker container prune -f

docker image prune -f

docker volume prune -f

docker network prune -f

echo "Docker Cleanup Completed."
```

---

# 🔍 Line-by-Line Explanation

## Display Start Message

```bash
echo "Starting Docker Cleanup..."
```

Prints a message indicating the cleanup process has started.

---

## Remove Stopped Containers

```bash
docker container prune -f
```

Removes all stopped containers.

The `-f` option skips the confirmation prompt.

---

## Remove Dangling Images

```bash
docker image prune -f
```

Deletes unused (dangling) images.

These are images that are no longer referenced by any container.

---

## Remove Unused Volumes

```bash
docker volume prune -f
```

Deletes Docker volumes that are not attached to any container.

---

## Remove Unused Networks

```bash
docker network prune -f
```

Deletes networks that are no longer used by any container.

---

## Completion Message

```bash
echo "Docker Cleanup Completed."
```

Displays a success message.

---

# ⭐ Sample Output

```
Starting Docker Cleanup...

Deleted Containers...

Deleted Images...

Deleted Volumes...

Deleted Networks...

Docker Cleanup Completed.
```

---

# ⭐ Check Docker Disk Usage Before Cleanup

```bash
docker system df
```

Example

```
TYPE            TOTAL     ACTIVE     SIZE

Images            20        5        8GB

Containers        15        2        1GB

Volumes           12        4        3GB
```

---

# ⭐ Production Version

Clean everything that is unused:

```bash
docker system prune -af
```

### Meaning

```
-a
```

Remove all unused images (not just dangling ones).

```
-f
```

Force cleanup without asking for confirmation.

> ⚠️ **Use this command carefully in production.** It removes all unused images, so future deployments may need to download or rebuild them again.

---

# ⭐ Remove Build Cache

```bash
docker builder prune -f
```

Removes Docker build cache and frees additional disk space.

---

# ⭐ Complete Production Cleanup

```bash
#!/bin/bash

echo "Cleaning Docker..."

docker system prune -af

docker volume prune -f

docker builder prune -f

echo "Docker cleanup completed."
```

---

# ⭐ Schedule Using Cron

Run every Sunday at 2:00 AM.

```bash
0 2 * * 0 /home/ubuntu/docker_cleanup.sh
```

---

# 💼 Real-Time DevOps Use Cases

## 1. Jenkins Build Server

Every build creates Docker images and containers.

Clean unused resources weekly to free storage.

---

## 2. Kubernetes Worker Node

Remove unused images after deployments.

This helps prevent worker nodes from running out of storage.

---

## 3. Development Server

Developers frequently build and delete containers.

Cleanup keeps the server organized.

---

## 4. CI/CD Pipeline

Automatically clean up after successful builds.

---

## 5. Shared Docker Hosts

Regular cleanup ensures all teams have sufficient disk space.

---

# ⭐ Best Practices

- Check disk usage before cleanup.
- Do not remove running containers.
- Schedule cleanup during low-traffic periods.
- Verify important images before deleting.
- Log cleanup activity.
- Test cleanup commands in a non-production environment first.

---

# 🎯 Interview Questions

## 1. Why is Docker cleanup necessary?

Docker resources accumulate over time and consume disk space. Cleanup helps maintain server health and prevents storage-related issues.

---

## 2. Which command removes stopped containers?

```bash
docker container prune
```

---

## 3. Which command removes dangling images?

```bash
docker image prune
```

---

## 4. Which command removes all unused Docker resources?

```bash
docker system prune -af
```

---

## 5. Which command checks Docker disk usage?

```bash
docker system df
```

---

## 6. Why should `docker system prune -af` be used carefully?

Because it removes all unused images, which may be required for future deployments and can increase deployment time if they must be downloaded again.

---

## 7. How would you improve this script for production?

- Log cleanup results.
- Send email or Slack notifications.
- Check disk usage before and after cleanup.
- Schedule during maintenance windows.
- Exclude critical resources if necessary.

---

# ⭐ Commands Used

| Command | Purpose |
|----------|----------|
| `docker container prune` | Remove stopped containers |
| `docker image prune` | Remove dangling images |
| `docker volume prune` | Remove unused volumes |
| `docker network prune` | Remove unused networks |
| `docker builder prune` | Remove build cache |
| `docker system prune -af` | Remove all unused Docker resources |
| `docker system df` | Display Docker disk usage |
| `cron` | Schedule cleanup |

---

# ✅ Script #9 Completed

Topics Revised

✔ Docker Containers

✔ Docker Images

✔ Docker Volumes

✔ Docker Networks

✔ Docker Build Cache

✔ Docker Cleanup

✔ Cron Jobs

✔ Disk Management

✔ Production Automation

# Interview Scenario
Interviewer:

"Your Docker server has reached 95% disk utilization because of old images and stopped containers. What would you do?"

Answer:

Check overall disk usage:

df -h

Check Docker's storage usage:

docker system df

List containers:

docker ps -a

Remove stopped containers:

docker container prune

Remove unused images:

docker image prune

Remove unused volumes:

docker volume prune

If appropriate, perform a complete cleanup:

docker system prune -af

Verify the available disk space again:

df -h

This structured troubleshooting process is commonly expected in Docker and DevOps interviews.

# 🚀 Production Script 10 - Docker Container Health Check Script

# 📖 What is a Docker Container Health Check Script?

A Docker Container Health Check Script monitors whether a Docker container is running.

If the container is stopped, the script automatically starts or restarts it and logs the action.

This minimizes downtime and ensures critical applications remain available.

---

# 🎯 Why Do DevOps Engineers Use It?

Production servers run many important containers:

- Nginx
- Jenkins
- SonarQube
- MySQL
- Redis
- Spring Boot Applications
- Node.js Applications

If a container stops unexpectedly:

- Websites become unavailable.
- CI/CD pipelines fail.
- APIs stop responding.
- Users experience downtime.

Instead of manually checking containers, DevOps Engineers automate the process.

---

# 📝 Complete Script

```bash
#!/bin/bash

CONTAINER="jenkins"

echo "Checking container: $CONTAINER"

if docker ps --format "{{.Names}}" | grep -w "$CONTAINER" > /dev/null
then
    echo "$CONTAINER is running."
else
    echo "$CONTAINER is stopped."

    echo "Starting container..."

    docker start $CONTAINER

    echo "$CONTAINER started successfully."
fi
```

---

# 🔍 Line-by-Line Explanation

## Container Name

```bash
CONTAINER="jenkins"
```

Stores the container name in a variable.

You can replace:

```
jenkins
```

with

```
nginx
redis
mysql
sonarqube
myapp
```

---

## Display Message

```bash
echo "Checking container: $CONTAINER"
```

---

## List Running Containers

```bash
docker ps
```

Example

```
CONTAINER ID

IMAGE

NAMES

9a4b5d

jenkins

jenkins

4fd1aa

nginx

nginx
```

---

## Display Only Container Names

```bash
docker ps --format "{{.Names}}"
```

Example

```
jenkins

nginx

redis
```

---

## Search for Container

```bash
grep -w "$CONTAINER"
```

Checks whether the specified container is currently running.

The `-w` option matches the complete container name.

---

## Running Container

```bash
echo "$CONTAINER is running."
```

---

## Stopped Container

```bash
docker start $CONTAINER
```

Starts the stopped container.

---

## Confirmation

```bash
echo "$CONTAINER started successfully."
```

---

# ⭐ Sample Output

## Running

```
Checking container: jenkins

jenkins is running.
```

---

## Stopped

```
Checking container: jenkins

jenkins is stopped.

Starting container...

jenkins started successfully.
```

---

# ⭐ Improved Production Version

Verify that the container actually started.

```bash
#!/bin/bash

CONTAINER="jenkins"

if docker ps --format "{{.Names}}" | grep -w "$CONTAINER" > /dev/null
then
    echo "$CONTAINER is already running."

else

    docker start $CONTAINER

    sleep 5

    if docker ps --format "{{.Names}}" | grep -w "$CONTAINER" > /dev/null
    then
        echo "$CONTAINER restarted successfully."
    else
        echo "Failed to start $CONTAINER."
    fi

fi
```

The `sleep 5` command waits 5 seconds before verifying the container status.

---

# ⭐ Monitor Multiple Containers

```bash
#!/bin/bash

containers=("jenkins" "nginx" "redis")

for container in "${containers[@]}"
do

    if docker ps --format "{{.Names}}" | grep -w "$container" > /dev/null
    then
        echo "$container is running."

    else

        echo "$container stopped."

        docker start $container

        echo "$container restarted."

    fi

done
```

---

# ⭐ Save Logs

```bash
echo "$(date): Restarted $CONTAINER container." >> docker_health.log
```

Example

```
Mon Jul 14 10:00:12

Restarted jenkins container.
```

---

# ⭐ Schedule Using Cron

Run every minute.

```bash
* * * * * /home/ubuntu/docker_health.sh
```

---

# 💼 Real-Time DevOps Use Cases

## 1. Jenkins Container

Automatically restart Jenkins if the container stops.

---

## 2. Nginx Container

Ensure the web server container remains available.

---

## 3. Redis Container

Restart Redis automatically to reduce downtime.

---

## 4. CI/CD Environment

Monitor build containers used during deployments.

---

## 5. Development Servers

Keep essential development services running.

---

# ⭐ Best Practices

- Verify container status before restarting.
- Log all restart events.
- Send alerts after repeated failures.
- Monitor only critical containers.
- Avoid restarting containers continuously if they repeatedly fail.
- Investigate the root cause of crashes.

---

# 🎯 Interview Questions

## 1. Which command displays running Docker containers?

```bash
docker ps
```

---

## 2. Which command displays all containers?

```bash
docker ps -a
```

---

## 3. Which command starts a stopped container?

```bash
docker start container_name
```

---

## 4. Which command restarts a container?

```bash
docker restart container_name
```

---

## 5. How do you check if a specific container is running?

```bash
docker ps --format "{{.Names}}" | grep -w container_name
```

---

## 6. Why should you verify the restart?

Because the container may fail to start due to application errors, configuration issues, or missing dependencies.

---

## 7. How would you improve this script for production?

- Check container health status.
- Log restart events.
- Send email or Slack notifications.
- Monitor multiple containers.
- Retry restart before sending alerts.
- Integrate with Prometheus or Grafana.

---

# ⭐ Commands Used

| Command | Purpose |
|----------|----------|
| `docker ps` | List running containers |
| `docker ps -a` | List all containers |
| `docker start` | Start a container |
| `docker restart` | Restart a container |
| `grep -w` | Match exact container name |
| `sleep` | Wait before verification |
| `cron` | Schedule monitoring |

---

# ✅ Script #10 Completed

Topics Revised

✔ Docker Containers

✔ Variables

✔ if Statement

✔ grep

✔ docker ps

✔ docker start

✔ Arrays

✔ for Loop

✔ Logging

✔ Cron Jobs

✔ Docker Monitoring

✔ Production Automation

# Interview Scenario
Interviewer:

"Your Jenkins Docker container stopped unexpectedly at midnight. No one noticed until developers reported build failures the next morning. How would you prevent this?"

Answer:

I would automate container monitoring:

Periodically check the container status using:

docker ps

If the container is not running:

docker start jenkins
Verify that the container started successfully.
Log the restart event.
Send an email or Slack notification if the restart fails.
Schedule the script using Cron (or use a monitoring solution like Prometheus + Alertmanager for larger environments).

This reduces downtime and ensures faster recovery from unexpected container failures.

# 🚀 Production Script 11 - Kubernetes Pod Health Check Script

# 📖 What is a Kubernetes Pod Health Check Script?

A Kubernetes Pod Health Check Script monitors the status of pods running inside a Kubernetes cluster.

If a pod is not in the **Running** state, the script alerts the administrator or restarts the Deployment.

This helps maintain application availability.

---

# 🎯 Why Do DevOps Engineers Use It?

Applications deployed on Kubernetes run inside Pods.

Sometimes Pods enter states like:

- CrashLoopBackOff
- Error
- Pending
- ImagePullBackOff
- OOMKilled

These issues affect application availability.

Instead of manually checking pods, DevOps Engineers automate pod monitoring.

---

# 📝 Complete Script

```bash
#!/bin/bash

NAMESPACE="default"

echo "Checking Pod Status..."

kubectl get pods -n $NAMESPACE
```

---

# 🔍 Line-by-Line Explanation

## Namespace

```bash
NAMESPACE="default"
```

Specifies the namespace where pods are running.

Examples:

```
default

dev

qa

production
```

---

## Display Message

```bash
echo "Checking Pod Status..."
```

---

## Display Pod Status

```bash
kubectl get pods -n $NAMESPACE
```

Example

```
NAME                    READY   STATUS

frontend-6d7f9          1/1     Running

backend-74dd9           1/1     Running

redis-65ff2             0/1     CrashLoopBackOff
```

---

# ⭐ Production Version

Check whether any pod is **not Running**.

```bash
#!/bin/bash

NAMESPACE="default"

pods=$(kubectl get pods -n $NAMESPACE --no-headers)

echo "$pods" | while read line
do

    pod=$(echo $line | awk '{print $1}')

    status=$(echo $line | awk '{print $3}')

    if [ "$status" != "Running" ]
    then
        echo "Pod: $pod Status: $status"

    fi

done
```

---

# 🔍 Explanation

## Remove Header

```bash
--no-headers
```

Displays only pod information.

---

## Read Each Pod

```bash
while read line
```

Processes one pod at a time.

---

## Extract Pod Name

```bash
awk '{print $1}'
```

Gets

```
frontend

backend

redis
```

---

## Extract Status

```bash
awk '{print $3}'
```

Gets

```
Running

Pending

CrashLoopBackOff
```

---

## Compare Status

```bash
if [ "$status" != "Running" ]
```

If the pod is not running, display a warning.

---

# ⭐ Sample Output

Healthy Cluster

```
Checking Pod Status...

All Pods Running
```

---

Problem

```
Pod: redis-65ff2

Status: CrashLoopBackOff
```

---

# ⭐ Restart Deployment

Instead of restarting a Pod directly, restart the Deployment.

```bash
kubectl rollout restart deployment my-app
```

This creates new Pods in a controlled way.

---

# ⭐ Check Logs

When a Pod is unhealthy:

```bash
kubectl logs pod-name
```

Example

```bash
kubectl logs redis-65ff2
```

---

# ⭐ Describe Pod

```bash
kubectl describe pod pod-name
```

Example

```bash
kubectl describe pod redis-65ff2
```

Useful for checking:

- Events
- Image pull errors
- Scheduling issues
- Resource limits

---

# ⭐ Save Logs

```bash
echo "$(date): Pod redis CrashLoopBackOff" >> pod_health.log
```

---

# ⭐ Schedule Using Cron

Run every 5 minutes.

```bash
*/5 * * * * /home/ubuntu/pod_health.sh
```

---

# 💼 Real-Time DevOps Use Cases

## 1. Production Cluster

Monitor all production application pods.

---

## 2. CI/CD Pipeline

Verify that new Pods are running after deployment.

---

## 3. Nightly Monitoring

Run automatically every few minutes and notify the team if a pod becomes unhealthy.

---

## 4. High Availability

Ensure replicas remain healthy after upgrades or scaling operations.

---

# ⭐ Best Practices

- Do not delete or restart Pods unnecessarily.
- Investigate logs before restarting.
- Restart Deployments instead of individual Pods when appropriate.
- Configure Liveness and Readiness Probes.
- Use monitoring tools such as Prometheus and Grafana.
- Send alerts for unhealthy Pods.

---

# 🎯 Interview Questions

## 1. Which command displays Pods?

```bash
kubectl get pods
```

---

## 2. Which command displays Pods in a namespace?

```bash
kubectl get pods -n namespace
```

---

## 3. Which command shows Pod logs?

```bash
kubectl logs pod-name
```

---

## 4. Which command shows detailed Pod information?

```bash
kubectl describe pod pod-name
```

---

## 5. Should we restart Pods directly?

Normally, **No**.

Pods are managed by Deployments or ReplicaSets.

Restart the Deployment instead.

---

## 6. Which command restarts a Deployment?

```bash
kubectl rollout restart deployment deployment-name
```

---

## 7. How would you troubleshoot a Pod in CrashLoopBackOff?

1. Check Pod status:
   ```bash
   kubectl get pods
   ```
2. Check logs:
   ```bash
   kubectl logs pod-name
   ```
3. Describe the Pod:
   ```bash
   kubectl describe pod pod-name
   ```
4. Verify image, environment variables, ConfigMaps, Secrets, and resource limits.
5. Restart the Deployment if necessary.

---

# ⭐ Commands Used

| Command | Purpose |
|----------|----------|
| `kubectl get pods` | List Pods |
| `kubectl logs` | View Pod logs |
| `kubectl describe pod` | View Pod details |
| `kubectl rollout restart deployment` | Restart Deployment |
| `awk` | Extract fields |
| `while read` | Process each Pod |
| `cron` | Schedule monitoring |

---

# ✅ Script #11 Completed

Topics Revised

✔ Kubernetes Pods

✔ Deployments

✔ Namespaces

✔ kubectl

✔ awk

✔ while Loop

✔ Logging

✔ Cron Jobs

✔ Kubernetes Monitoring

✔ Production Automation

# Interview Scenario
Interviewer:

"Your application is unavailable, and kubectl get pods shows one Pod in CrashLoopBackOff. What steps would you take?"

Answer:

Check the Pod status:

kubectl get pods

View the application logs:

kubectl logs <pod-name>

Describe the Pod to inspect events:

kubectl describe pod <pod-name>
Check for:
Incorrect image
Missing ConfigMaps or Secrets
Environment variable issues
Resource limits
Liveness/Readiness probe failures
Fix the root cause.

Restart the Deployment if required:

kubectl rollout restart deployment <deployment-name>

# 🚀 Production Script 12 - Deployment Automation Script

# 📖 What is a Deployment Automation Script?

A Deployment Automation Script automates the process of deploying an application.

Instead of manually executing multiple commands, the script performs the deployment steps in sequence, reducing manual effort and minimizing deployment errors.

This is commonly used in Jenkins, GitHub Actions, GitLab CI/CD, and Azure DevOps pipelines.

---

# 🎯 Why Do DevOps Engineers Use It?

Without automation, deployment involves multiple manual steps:

- Pull latest source code
- Build the application
- Run tests
- Build Docker image
- Push image to Docker Registry
- Update Kubernetes Deployment
- Verify deployment

Automating these tasks makes deployments:

- Faster
- Consistent
- Repeatable
- Less error-prone

---

# 📝 Complete Script

```bash
#!/bin/bash

echo "Starting Deployment..."

cd /home/ubuntu/myapp || exit 1

git pull origin main

mvn clean package

docker build -t myapp:latest .

docker stop myapp || true

docker rm myapp || true

docker run -d --name myapp -p 8080:8080 myapp:latest

echo "Deployment Completed Successfully."
```

---

# 🔍 Line-by-Line Explanation

## Display Message

```bash
echo "Starting Deployment..."
```

Displays the deployment start message.

---

## Change Directory

```bash
cd /home/ubuntu/myapp || exit 1
```

Moves to the application directory.

The `|| exit 1` ensures the script stops immediately if the directory does not exist.

---

## Pull Latest Code

```bash
git pull origin main
```

Downloads the latest code from the Git repository.

---

## Build Application

```bash
mvn clean package
```

For Java applications:

- Cleans previous builds
- Compiles source code
- Runs tests
- Creates a JAR/WAR package

---

## Build Docker Image

```bash
docker build -t myapp:latest .
```

Creates a new Docker image.

Options:

```
-t
```

Assigns a tag.

```
.
```

Uses the current directory as the Docker build context.

---

## Stop Existing Container

```bash
docker stop myapp || true
```

Stops the running container.

`|| true` prevents the script from failing if the container is not already running.

---

## Remove Old Container

```bash
docker rm myapp || true
```

Deletes the old container before creating a new one.

---

## Start New Container

```bash
docker run -d --name myapp -p 8080:8080 myapp:latest
```

Options:

- `-d` → Run in detached mode.
- `--name` → Assign a container name.
- `-p` → Map host port to container port.

---

## Display Success Message

```bash
echo "Deployment Completed Successfully."
```

---

# ⭐ Sample Output

```
Starting Deployment...

Already up to date.

BUILD SUCCESS

Successfully built image

Stopping old container...

Removing old container...

Starting new container...

Deployment Completed Successfully.
```

---

# ⭐ Kubernetes Deployment Version

Instead of restarting a Docker container, update Kubernetes.

```bash
kubectl apply -f deployment.yaml

kubectl rollout status deployment/myapp
```

This applies the new configuration and waits until the rollout is complete.

---

# ⭐ Verify Deployment

Docker:

```bash
docker ps
```

Kubernetes:

```bash
kubectl get pods

kubectl get deployment
```

Application:

```bash
curl http://localhost:8080
```

---

# ⭐ Production Version

```bash
#!/bin/bash

set -e

echo "Starting Deployment..."

git pull origin main

mvn clean package

docker build -t myapp:latest .

docker stop myapp || true

docker rm myapp || true

docker run -d --name myapp -p 8080:8080 myapp:latest

echo "Deployment Successful."
```

### Why `set -e`?

If any command fails, the script stops immediately instead of continuing with a broken deployment.

---

# ⭐ Save Logs

```bash
echo "$(date): Deployment Successful" >> deployment.log
```

---

# ⭐ Real CI/CD Pipeline Flow

```
Developer

↓

Git Push

↓

Jenkins / GitHub Actions / Azure DevOps

↓

Git Pull

↓

Build Application

↓

Run Tests

↓

Build Docker Image

↓

Push Image to Docker Hub / ECR / ACR

↓

Update Kubernetes Deployment

↓

Verify Deployment

↓

Deployment Complete
```

---

# 💼 Real-Time DevOps Use Cases

## 1. Jenkins Pipeline

Execute the deployment script after a successful build.

---

## 2. GitHub Actions

Run the script automatically after code is merged into the main branch.

---

## 3. Azure DevOps Pipeline

Deploy applications to Kubernetes or Docker hosts.

---

## 4. Production Releases

Standardize deployment across environments (Dev, QA, Production).

---

# ⭐ Best Practices

- Use `set -e` to stop on failures.
- Validate prerequisites before deployment.
- Run automated tests.
- Verify application health after deployment.
- Log deployment events.
- Roll back automatically if deployment fails.

---

# 🎯 Interview Questions

## 1. Why automate deployments?

Automation reduces manual errors, speeds up releases, and ensures consistent deployments.

---

## 2. What is the purpose of `git pull`?

It retrieves the latest code from the remote repository.

---

## 3. Why use `docker stop` before `docker run`?

To stop the old application instance before starting the new version.

---

## 4. What does `|| true` do?

It ignores errors from the preceding command, allowing the script to continue.

Example: if the container does not exist, the script won't fail.

---

## 5. Why use `set -e`?

It exits the script immediately if any command returns a non-zero exit status.

This prevents partial or inconsistent deployments.

---

## 6. How do you verify a Kubernetes deployment?

```bash
kubectl rollout status deployment/myapp

kubectl get pods
```

---

## 7. How would you improve this script for production?

- Add health checks after deployment.
- Push Docker images to a registry.
- Update Kubernetes manifests automatically.
- Implement rollback on failure.
- Send deployment notifications.
- Log all deployment activities.

---

# ⭐ Commands Used

| Command | Purpose |
|----------|----------|
| `git pull` | Get latest source code |
| `mvn clean package` | Build Java application |
| `docker build` | Build Docker image |
| `docker stop` | Stop running container |
| `docker rm` | Remove old container |
| `docker run` | Start new container |
| `kubectl apply` | Apply Kubernetes configuration |
| `kubectl rollout status` | Monitor deployment rollout |
| `set -e` | Stop script on error |

---

# ✅ Script #12 Completed

Topics Revised

✔ Git

✔ Maven

✔ Docker

✔ Kubernetes

✔ CI/CD

✔ Error Handling

✔ Deployment Automation

✔ Logging

✔ Health Verification

✔ Production Best Practices

# Interview Scenario
Interviewer:

"A developer has merged code into the main branch. Explain what happens in your deployment pipeline."

Answer:
1. The CI/CD pipeline is triggered automatically.
2. The latest code is fetched from Git.
3. The application is built (mvn clean package).
4. Automated tests are executed.
5. A new Docker image is built.
6. The image is pushed to a container registry (Docker Hub, Amazon ECR, or Azure Container Registry).
7. Kubernetes deployment manifests are updated with the new image version.
8. Kubernetes performs a rolling update.

9. The deployment is verified using:

kubectl rollout status deployment/myapp
10. If verification succeeds, the deployment is complete. If it fails, the deployment is rolled back.

This is a production-standard CI/CD workflow and closely matches what you've already practiced with Azure DevOps, Docker, Kubernetes, and Jenkins.

# 🚀 Production Script 13 - Jenkins Backup Script

# 📖 What is a Jenkins Backup Script?

A Jenkins Backup Script automatically creates a backup of the Jenkins home directory (`JENKINS_HOME`).

The backup includes:

- Jobs
- Pipelines
- Plugins
- Configuration files
- Build history
- Credentials (encrypted)
- User configurations

Regular backups ensure Jenkins can be restored quickly in case of server failure or accidental data loss.

---

# 🎯 Why Do DevOps Engineers Use It?

Jenkins is a critical CI/CD tool.

If the Jenkins server crashes or the disk fails:

- Pipelines are lost.
- Job configurations disappear.
- Plugins need reinstallation.
- Build history is unavailable.

Automated backups prevent these problems.

---

# 📝 Complete Script

```bash
#!/bin/bash

JENKINS_HOME="/var/lib/jenkins"

BACKUP_DIR="/backup/jenkins"

DATE=$(date +%Y-%m-%d_%H-%M-%S)

mkdir -p $BACKUP_DIR

echo "Starting Jenkins Backup..."

tar -czf $BACKUP_DIR/jenkins_backup_$DATE.tar.gz $JENKINS_HOME

echo "Backup Completed Successfully."

echo "Backup Location: $BACKUP_DIR"
```

---

# 🔍 Line-by-Line Explanation

## Jenkins Home Directory

```bash
JENKINS_HOME="/var/lib/jenkins"
```

This is the default directory where Jenkins stores all its data.

Common locations:

```
Ubuntu

/var/lib/jenkins

Docker

/var/jenkins_home
```

---

## Backup Directory

```bash
BACKUP_DIR="/backup/jenkins"
```

Location where backups will be stored.

---

## Generate Timestamp

```bash
DATE=$(date +%Y-%m-%d_%H-%M-%S)
```

Example

```
2026-07-14_02-00-00
```

This ensures each backup has a unique filename.

---

## Create Backup Directory

```bash
mkdir -p $BACKUP_DIR
```

Creates the backup directory if it does not already exist.

---

## Display Message

```bash
echo "Starting Jenkins Backup..."
```

---

## Create Compressed Backup

```bash
tar -czf $BACKUP_DIR/jenkins_backup_$DATE.tar.gz $JENKINS_HOME
```

Options:

```
-c
```

Create archive.

```
-z
```

Compress using gzip.

```
-f
```

Specify the output file.

---

## Success Message

```bash
echo "Backup Completed Successfully."
```

---

## Display Backup Location

```bash
echo "Backup Location: $BACKUP_DIR"
```

---

# ⭐ Sample Output

```
Starting Jenkins Backup...

Backup Completed Successfully.

Backup Location:

/backup/jenkins
```

---

# ⭐ Verify Backup

List backup files:

```bash
ls -lh /backup/jenkins
```

Example

```
jenkins_backup_2026-07-14_02-00-00.tar.gz
```

---

# ⭐ Restore Jenkins Backup

Stop Jenkins first:

```bash
sudo systemctl stop jenkins
```

Extract the backup:

```bash
tar -xzf jenkins_backup_2026-07-14_02-00-00.tar.gz -C /
```

Start Jenkins again:

```bash
sudo systemctl start jenkins
```

---

# ⭐ Delete Old Backups

Delete backups older than 30 days:

```bash
find $BACKUP_DIR -name "*.tar.gz" -mtime +30 -delete
```

---

# ⭐ Production Version

```bash
#!/bin/bash

JENKINS_HOME="/var/lib/jenkins"

BACKUP_DIR="/backup/jenkins"

mkdir -p $BACKUP_DIR

DATE=$(date +%F_%H-%M-%S)

tar -czf $BACKUP_DIR/jenkins_$DATE.tar.gz $JENKINS_HOME

find $BACKUP_DIR -name "*.tar.gz" -mtime +30 -delete

echo "Backup Successful"
```

This version:

- Creates the backup.
- Automatically removes backups older than 30 days.

---

# ⭐ Upload Backup to Cloud (Example)

After creating the backup, upload it to Amazon S3:

```bash
aws s3 cp jenkins_backup.tar.gz s3://my-backup-bucket/
```

Or upload to Azure Blob Storage using the Azure CLI.

This protects backups even if the Jenkins server is lost.

---

# ⭐ Schedule Using Cron

Run every day at 1:00 AM.

```bash
0 1 * * * /home/ubuntu/jenkins_backup.sh
```

---

# 💼 Real-Time DevOps Use Cases

## 1. Daily Jenkins Backup

Create a backup every night.

---

## 2. Before Jenkins Upgrade

Take a backup before upgrading Jenkins or plugins.

---

## 3. Disaster Recovery

Restore Jenkins after a server failure.

---

## 4. Cloud Migration

Back up Jenkins before moving it to another server or cloud provider.

---

# ⭐ Best Practices

- Schedule backups daily.
- Store backups on another server or cloud storage.
- Encrypt backup files if they contain sensitive information.
- Periodically test restoration.
- Remove old backups automatically.
- Monitor backup success.

---

# 🎯 Interview Questions

## 1. What is `JENKINS_HOME`?

It is the directory where Jenkins stores jobs, plugins, configurations, credentials, and build history.

---

## 2. Which command creates a compressed backup?

```bash
tar -czf
```

---

## 3. Why include the date in the backup filename?

To create unique backups and prevent overwriting previous backups.

---

## 4. How do you restore a Jenkins backup?

1. Stop Jenkins.
2. Extract the backup into `JENKINS_HOME`.
3. Start Jenkins.
4. Verify jobs and pipelines.

---

## 5. Why store backups outside the Jenkins server?

If the server fails completely, local backups may also be lost. Storing backups on cloud storage or another server provides disaster recovery.

---

## 6. How do you automate Jenkins backups?

Using Cron.

Example:

```bash
0 1 * * * /home/ubuntu/jenkins_backup.sh
```

---

## 7. How would you improve this script for production?

- Verify backup success.
- Upload backups to cloud storage.
- Encrypt backup archives.
- Log backup activities.
- Send email or Slack notifications.
- Remove old backups automatically.

---

# ⭐ Commands Used

| Command | Purpose |
|----------|----------|
| `tar -czf` | Create compressed archive |
| `mkdir -p` | Create directory if it doesn't exist |
| `date` | Generate timestamp |
| `find` | Locate old backups |
| `-mtime` | Filter by age |
| `-delete` | Remove old backups |
| `aws s3 cp` | Upload backup to Amazon S3 |
| `cron` | Schedule backups |

---

# ✅ Script #13 Completed

Topics Revised

✔ Jenkins

✔ tar

✔ Backup Automation

✔ Restore Process

✔ Cloud Backup

✔ Cron Jobs

✔ Disaster Recovery

✔ Production Automation

# Interview Scenario
Interviewer:

"Your Jenkins server crashes because of a disk failure. How will you recover it?"

Answer:
1. Provision a new server.
2. Install the same Jenkins version.

3. Stop the Jenkins service:

sudo systemctl stop jenkins

4. Restore the backup into JENKINS_HOME:

tar -xzf jenkins_backup.tar.gz -C /

5. Ensure the ownership and permissions are correct:

sudo chown -R jenkins:jenkins /var/lib/jenkins

6. Start Jenkins:

sudo systemctl start jenkins
7. Verify that:
Jobs are present.
Pipelines work.
Plugins are loaded.
Credentials are available.

This is a standard disaster recovery process that interviewers expect DevOps engineers to understand.

# 🚀 Production Script 14 - SSL Certificate Expiry Check Script

# 📖 What is an SSL Certificate Expiry Check Script?

An SSL Certificate Expiry Check Script checks the expiry date of a website's SSL/TLS certificate.

If the certificate is close to expiring (for example, within 30 days), the script generates an alert.

This helps administrators renew certificates before they expire.

---

# 🎯 Why Do DevOps Engineers Use It?

Most production applications use HTTPS.

Examples:

- Banking Applications
- E-commerce Websites
- Jenkins
- Kubernetes Ingress
- Company Portals
- REST APIs

If an SSL certificate expires:

- Browsers show security warnings.
- Users cannot trust the website.
- API calls may fail.
- Business operations may be affected.

Therefore, SSL expiry monitoring is automated.

---

# 📝 Complete Script

```bash
#!/bin/bash

DOMAIN="google.com"

echo "Checking SSL Certificate for $DOMAIN..."

echo | openssl s_client -connect $DOMAIN:443 -servername $DOMAIN 2>/dev/null \
| openssl x509 -noout -dates
```

---

# 🔍 Line-by-Line Explanation

## Store Domain Name

```bash
DOMAIN="google.com"
```

Replace with your own domain.

Examples:

```
example.com

company.com

jenkins.company.com

api.company.com
```

---

## Display Message

```bash
echo "Checking SSL Certificate for $DOMAIN..."
```

---

## Connect to Website

```bash
openssl s_client
```

Creates an SSL/TLS connection to the server.

---

## Specify Server

```bash
-connect $DOMAIN:443
```

Connects to HTTPS port 443.

---

## Server Name Indication (SNI)

```bash
-servername $DOMAIN
```

Ensures the correct SSL certificate is returned when multiple websites share the same IP.

---

## Hide Error Messages

```bash
2>/dev/null
```

Suppresses unnecessary OpenSSL output.

---

## Display Certificate Dates

```bash
openssl x509 -noout -dates
```

Shows:

- Certificate start date
- Certificate expiry date

---

# ⭐ Sample Output

```
Checking SSL Certificate for google.com...

notBefore=Jul 10 00:00:00 2026 GMT

notAfter=Oct 02 23:59:59 2026 GMT
```

---

# ⭐ Check Only Expiry Date

```bash
echo | openssl s_client -connect google.com:443 -servername google.com 2>/dev/null \
| openssl x509 -noout -enddate
```

Example

```
notAfter=Oct 02 23:59:59 2026 GMT
```

---

# ⭐ Production Version

```bash
#!/bin/bash

DOMAIN="example.com"

EXPIRY=$(echo | openssl s_client -connect $DOMAIN:443 -servername $DOMAIN 2>/dev/null \
| openssl x509 -noout -enddate | cut -d= -f2)

echo "Certificate Expiry Date: $EXPIRY"
```

This stores the expiry date in a variable for further processing or alerting.

---

# ⭐ Save Results to Log

```bash
echo "$(date): $DOMAIN expires on $EXPIRY" >> ssl_check.log
```

Example

```
Mon Jul 14

example.com expires on Oct 02 2026
```

---

# ⭐ Monitor Multiple Domains

```bash
#!/bin/bash

domains=("google.com" "github.com" "openai.com")

for domain in "${domains[@]}"
do

echo "Checking $domain..."

echo | openssl s_client -connect $domain:443 -servername $domain 2>/dev/null \
| openssl x509 -noout -enddate

done
```

---

# ⭐ Schedule Using Cron

Check certificates every day at 9:00 AM.

```bash
0 9 * * * /home/ubuntu/ssl_check.sh
```

---

# 💼 Real-Time DevOps Use Cases

## 1. Company Websites

Monitor SSL certificates for public websites.

---

## 2. API Servers

Ensure API endpoints continue using valid HTTPS certificates.

---

## 3. Kubernetes Ingress

Verify certificates used by Ingress Controllers.

---

## 4. Jenkins

Monitor SSL certificates used by Jenkins dashboards.

---

## 5. Load Balancers

Check certificates installed on Nginx, Apache, or HAProxy.

---

# ⭐ Best Practices

- Check certificates daily.
- Alert administrators at least 30 days before expiry.
- Monitor all production domains.
- Log certificate expiry dates.
- Automate certificate renewal where possible (e.g., Let's Encrypt with Certbot).

---

# 🎯 Interview Questions

## 1. Why do we monitor SSL certificates?

To prevent service outages caused by expired certificates and ensure secure HTTPS communication.

---

## 2. Which command checks an SSL certificate?

```bash
openssl s_client
```

---

## 3. Which command displays certificate dates?

```bash
openssl x509 -noout -dates
```

---

## 4. What does `-enddate` display?

It displays only the certificate expiry date.

---

## 5. Why do we use `-servername`?

To support Server Name Indication (SNI), ensuring the correct certificate is returned when multiple domains share the same IP address.

---

## 6. How do you automate SSL certificate monitoring?

Using Cron.

Example:

```bash
0 9 * * * /home/ubuntu/ssl_check.sh
```

---

## 7. How would you improve this script for production?

- Calculate the number of days remaining until expiry.
- Send email or Slack alerts if expiry is within a threshold (e.g., 30 days).
- Monitor multiple domains.
- Log results to a central monitoring system.
- Integrate with Prometheus or Nagios.

---

# ⭐ Commands Used

| Command | Purpose |
|----------|----------|
| `openssl s_client` | Establish SSL/TLS connection |
| `openssl x509` | Read certificate information |
| `-dates` | Show validity period |
| `-enddate` | Show certificate expiry date |
| `cut` | Extract expiry date |
| `cron` | Schedule automatic checks |

---

# ✅ Script #14 Completed

Topics Revised

✔ OpenSSL

✔ SSL Certificates

✔ HTTPS

✔ Variables

✔ Arrays

✔ for Loop

✔ Logging

✔ Cron Jobs

✔ Certificate Monitoring

✔ Production Automation

# Interview Scenario
Interviewer:

"Users report that they see a browser warning saying 'Your connection is not private.' How would you troubleshoot?"

Answer:

1. Verify the website is accessible:

curl -I https://example.com

2. Check the SSL certificate:

echo | openssl s_client -connect example.com:443 -servername example.com

3. Check the certificate expiry:

echo | openssl s_client -connect example.com:443 -servername example.com 2>/dev/null \
| openssl x509 -noout -dates
4. Confirm:
The certificate has not expired.
The certificate matches the domain.
The full certificate chain is installed correctly.
5. Renew or replace the certificate if required.
6. Reload the web server (Nginx/Apache) after installing the new certificate.

# 🚀 Production Script 15 - Complete Server Health Report Script

# 📖 What is a Server Health Report Script?

A Server Health Report Script collects important server information such as:

- Server Uptime
- CPU Usage
- Memory Usage
- Disk Usage
- Running Services
- Docker Status
- Kubernetes Status

It provides a quick overview of the server's health.

Instead of running multiple Linux commands manually, one script generates a complete health report.

---

# 🎯 Why Do DevOps Engineers Use It?

Production servers host critical applications.

Regular health checks help detect problems before users notice them.

Examples:

- Disk becoming full
- Memory running low
- High CPU utilization
- Docker service stopped
- Kubernetes Pods unhealthy

Instead of checking each component manually, a health report summarizes everything.

---

# 📝 Complete Script

```bash
#!/bin/bash

echo "====================================="
echo "      SERVER HEALTH REPORT"
echo "====================================="

echo
echo "Hostname:"
hostname

echo
echo "Date:"
date

echo
echo "Server Uptime:"
uptime

echo
echo "-------------------------------------"
echo "CPU Usage"
top -bn1 | grep "Cpu(s)"

echo
echo "-------------------------------------"
echo "Memory Usage"
free -h

echo
echo "-------------------------------------"
echo "Disk Usage"
df -h

echo
echo "-------------------------------------"
echo "Top 5 CPU Processes"
ps -eo pid,comm,%cpu --sort=-%cpu | head -6

echo
echo "-------------------------------------"
echo "Docker Status"

systemctl is-active docker

echo
echo "-------------------------------------"
echo "Running Containers"

docker ps

echo
echo "-------------------------------------"
echo "Kubernetes Nodes"

kubectl get nodes

echo
echo "-------------------------------------"
echo "Pods"

kubectl get pods -A

echo
echo "====================================="
echo "Report Generated Successfully"
echo "====================================="
```

---

# 🔍 Line-by-Line Explanation

## Display Report Title

```bash
echo "SERVER HEALTH REPORT"
```

Displays the report heading.

---

## Hostname

```bash
hostname
```

Shows the server name.

Example

```
prod-server-01
```

---

## Current Date

```bash
date
```

Displays the current system date and time.

---

## Server Uptime

```bash
uptime
```

Shows

- Running time
- Logged-in users
- Load Average

Example

```
10:35 up 20 days,
3 users,
load average: 0.42,0.35,0.30
```

---

## CPU Usage

```bash
top -bn1 | grep "Cpu(s)"
```

Displays current CPU utilization.

---

## Memory Usage

```bash
free -h
```

Displays:

- Total Memory
- Used Memory
- Free Memory
- Swap

---

## Disk Usage

```bash
df -h
```

Displays disk usage in a human-readable format.

---

## Top CPU Processes

```bash
ps -eo pid,comm,%cpu --sort=-%cpu | head -6
```

Displays the top five CPU-consuming processes.

---

## Docker Status

```bash
systemctl is-active docker
```

Shows

```
active

or

inactive
```

---

## Running Containers

```bash
docker ps
```

Lists all running containers.

---

## Kubernetes Nodes

```bash
kubectl get nodes
```

Displays cluster node status.

---

## Kubernetes Pods

```bash
kubectl get pods -A
```

Displays Pods across all namespaces.

---

# ⭐ Sample Output

```
SERVER HEALTH REPORT

Hostname

prod-server

Date

Mon Jul 14

Uptime

20 days

CPU

18%

Memory

45%

Disk

62%

Docker

active

Pods

Running
```

---

# ⭐ Save Report to File

```bash
./health_report.sh > report.txt
```

Example

```
report.txt
```

can be emailed or shared.

---

# ⭐ Production Version

```bash
REPORT="/tmp/server_health_$(date +%F).txt"

./health_report.sh > $REPORT

echo "Health Report Saved"

echo "$REPORT"
```

---

# ⭐ Email Report

```bash
mail -s "Daily Server Health Report" admin@example.com < report.txt
```

This sends the report automatically.

---

# ⭐ Schedule Using Cron

Run every day at 8 AM.

```bash
0 8 * * * /home/ubuntu/health_report.sh
```

---

# 💼 Real-Time DevOps Use Cases

## 1. Daily Health Report

Generate server health reports every morning.

---

## 2. Production Monitoring

Quickly identify resource issues.

---

## 3. Client Reporting

Share daily infrastructure status.

---

## 4. Cloud Servers

Monitor EC2, Azure VM, or Google Compute Engine instances.

---

## 5. Kubernetes Cluster

Check node and pod health regularly.

---

# ⭐ Best Practices

- Save reports to a file.
- Send reports through email or Slack.
- Include threshold-based alerts.
- Schedule using Cron.
- Integrate with Prometheus and Grafana for continuous monitoring.
- Review reports regularly.

---

# 🎯 Interview Questions

## 1. Why do we generate health reports?

To monitor server resources, detect issues early, and maintain system availability.

---

## 2. Which command checks server uptime?

```bash
uptime
```

---

## 3. Which command checks memory usage?

```bash
free -h
```

---

## 4. Which command checks disk usage?

```bash
df -h
```

---

## 5. Which command displays CPU usage?

```bash
top -bn1
```

---

## 6. Which command displays the top CPU-consuming processes?

```bash
ps -eo pid,comm,%cpu --sort=-%cpu | head
```

---

## 7. Which command lists running Docker containers?

```bash
docker ps
```

---

## 8. Which command displays Kubernetes Pods?

```bash
kubectl get pods -A
```

---

## 9. How would you improve this script for production?

- Add threshold-based alerts for CPU, memory, and disk usage.
- Log reports to a central location.
- Send email or Slack notifications.
- Export reports to HTML or PDF.
- Integrate with Prometheus, Grafana, or monitoring tools.

---

# ⭐ Commands Used

| Command | Purpose |
|----------|----------|
| `hostname` | Display server name |
| `date` | Show current date and time |
| `uptime` | Display system uptime |
| `top` | Display CPU usage |
| `free -h` | Display memory usage |
| `df -h` | Display disk usage |
| `ps` | Display running processes |
| `systemctl is-active` | Check Docker service |
| `docker ps` | List running containers |
| `kubectl get nodes` | Display Kubernetes nodes |
| `kubectl get pods -A` | Display Pods in all namespaces |
| `cron` | Schedule report generation |

---

# ✅ Script #15 Completed

Topics Revised

✔ Linux Monitoring

✔ CPU Monitoring

✔ Memory Monitoring

✔ Disk Monitoring

✔ Process Monitoring

✔ Docker Monitoring

✔ Kubernetes Monitoring

✔ Health Reporting

✔ Logging

✔ Cron Jobs

✔ Production Automation

#  Scenario Based Questions
### Scenario 1
Server disk suddenly became 100%.

How will you troubleshoot?

Answer

Step 1

df -h

Check which filesystem is full.

Step 2

du -sh /*

Find large directories.

Step 3

find /var/log -type f -name "*.log"

Check logs.

Step 4

docker system df

Check Docker storage.

Step 5

docker system prune -af

Remove unused Docker resources.

Step 6

Archive or clean old logs.

### Scenario 2

Jenkins service stopped.

What will you do?

systemctl status jenkins

journalctl -u jenkins

systemctl restart jenkins

systemctl enable jenkins

Check

Logs
Port usage
Disk space
Memory
### Scenario 3

Application is slow.

How do you troubleshoot?

Check

top

htop

free -h

df -h

uptime

ps -ef

Look for

High CPU
High Memory
Full Disk
Deadlocked Process
### Scenario 4

Docker server has no storage left.

Commands

docker system df

docker ps -a

docker images

docker volume ls

docker network ls

docker system prune -af
### Scenario 5

Developer says

"My container is not starting."

Commands

docker ps -a

docker logs container_name

docker inspect container_name

docker restart container_name
### Scenario 6

Pod is CrashLoopBackOff.

Commands

kubectl get pods

kubectl describe pod

kubectl logs pod

kubectl get events
### Scenario 7

Pod is Pending.

Possible Reasons

No Resources
Taints
PVC issue
Node unavailable

Commands

kubectl describe pod
### Scenario 8

Website is not opening.

Check

systemctl status nginx

systemctl status apache2

netstat -tulnp

ss -tulnp

curl localhost
### Scenario 9

Server is reachable through ping but website is inaccessible.

Possible reasons

Web server stopped
Firewall
Wrong port
SSL issue
Application crashed
### Scenario 10

Server is unreachable.

Check

Network

DNS

Gateway

Security Groups

Firewall

Routing

### Scenario 11

Memory utilization reached 95%.

Commands

free -h

top

ps aux --sort=-%mem

Restart leaking applications if required.

### Scenario 12

CPU usage reached 100%.

Commands

top

htop

ps -eo pid,comm,%cpu --sort=-%cpu
### Scenario 13

Someone deleted an important file.

What will you do?

Restore from backup.

### Scenario 14

SSL certificate expired.

Steps

Check

openssl s_client

openssl x509

Renew certificate.

Restart web server.

### Scenario 15

Deployment failed.

Check

Git Pull

Build

Docker

Image

Registry

Kubernetes

Logs

### Scenario 16

Git pull failed.

Possible reasons

Merge conflict
Authentication
Wrong branch
### Scenario 17

Build failed in Jenkins.

Check

Console Output

Workspace

Maven Logs

Permissions

### Scenario 18

Application running locally but not in Docker.

Check

Dockerfile

Ports

Environment Variables

Logs

### Scenario 19

Docker image is too large.

Solutions

Multi-stage builds

Smaller base image

Remove cache

### Scenario 20

Cron job is not running.

Check

crontab -l

systemctl status cron

Verify

Permissions

Paths

Logs

#  Advanced Shell Scripting for DevOps
### 1. grep (Global Regular Expression Print)
What is grep?

grep searches for text or patterns inside files.

It is one of the most frequently used Linux commands in DevOps.

Syntax
grep "text" filename

Example:

grep "ERROR" application.log

Output:

ERROR Database connection failed
Common Options
Ignore case
grep -i "error" app.log

Matches:

ERROR
error
Error
Show line numbers
grep -n "ERROR" app.log

Output:

15:ERROR Database Down
Count matches
grep -c "ERROR" app.log

Output:

12
Invert match
grep -v "INFO" app.log

Shows all lines except those containing INFO.

Recursive search
grep -r "password" /etc

Searches all files under /etc.

Production Example

Find failed logins:

grep "Failed password" /var/log/auth.log
### sed (Stream Editor)
What is sed?

sed edits text without opening the file in an editor.

Replace text
sed 's/dev/prod/' config.txt
Replace all occurrences
sed 's/dev/prod/g' config.txt
Modify the file directly
sed -i 's/dev/prod/g' config.txt
Delete a line
sed '3d' file.txt

Deletes line 3.

Production Example

Update image version in a Kubernetes manifest:

sed -i 's/v1.0/v2.0/' deployment.yaml
### awk
What is awk?

awk extracts and processes columns from structured text.

Example:

cat users.txt
John 25
Alice 30
Bob 28
Print first column
awk '{print $1}' users.txt

Output:

John
Alice
Bob
Print second column
awk '{print $2}' users.txt

Output:

25
30
28
Multiple columns
awk '{print $1,$2}'
Production Example

Show running pod names:

kubectl get pods | awk '{print $1}'
### xargs
What is xargs?

xargs takes input from another command and passes it as arguments to another command.

Example:

find . -name "*.log" | xargs rm

Deletes all .log files found.

Production Example

Remove stopped Docker containers:

docker ps -aq -f status=exited | xargs docker rm
### tee
What is tee?

tee writes output to both the terminal and a file.

Example:

ls | tee output.txt

Output appears on the screen and is also saved in output.txt.

Append instead of overwrite
ls | tee -a output.txt
Production Example

Save deployment logs:

./deploy.sh | tee deployment.log
### Here Document (Heredoc)
What is a Heredoc?

A Heredoc allows you to provide multiple lines of input directly within a script.

Example
cat << EOF
Hello
Welcome
DevOps
EOF

Output:

Hello
Welcome
DevOps
Create a configuration file
cat << EOF > config.txt
APP_NAME=myapp
PORT=8080
ENV=production
EOF
Production Example

Generate an Nginx configuration automatically.

### getopts
What is getopts?

getopts processes command-line options professionally.

Instead of:

./script.sh sahithi admin

Use:

./script.sh -u sahithi -p admin
Example
#!/bin/bash

while getopts u:p: option
do
    case $option in
        u) USER=$OPTARG ;;
        p) PASS=$OPTARG ;;
    esac
done

echo "User: $USER"
echo "Password: $PASS"

Run:

./script.sh -u sahithi -p Welcome123

Output:

User: sahithi
Password: Welcome123
### Production Logging Function

Instead of repeatedly writing:

echo "Deployment Started"

Create a reusable function:

log() {
    echo "$(date): $1"
}

Use it like:

log "Deployment Started"
log "Docker Build Completed"
log "Deployment Successful"

Output:

Mon Jul 13 10:00:00 Deployment Started
Mon Jul 13 10:02:00 Docker Build Completed
Mon Jul 13 10:05:00 Deployment Successful

# Useful Command Combinations
### Find large files
find / -type f -size +100M
### Top memory-consuming processes
ps -eo pid,comm,%mem --sort=-%mem | head
### Count ERROR messages
grep -c "ERROR" app.log
### Replace all occurrences
sed -i 's/http/https/g' config.txt
### Extract first column
awk '{print $1}'
### Save output while displaying it
kubectl get pods | tee pods.txt
# Mini DevOps Exercises
### 1. Exercise 1

Count failed login attempts:

grep -c "Failed" /var/log/auth.log
### 2. Exercise 2

Replace image version:

sed -i 's/v1/v2/' deployment.yaml
### 3. Exercise 3

Extract all pod names:

kubectl get pods | awk '{print $1}'
### 4. Exercise 4

Delete all .tmp files:

find . -name "*.tmp" | xargs rm
### 5. Exercise 5

Save deployment logs:

./deploy.sh | tee deploy.log

# Interview Questions
### 1. What is grep used for?

Searching text or patterns inside files.

### 2. Difference between grep and find?
grep searches inside file contents.
find searches for files and directories.
### 3. What is sed?

A stream editor used to search, replace, insert, or delete text.

### 4. What is awk?

A text-processing tool mainly used to extract and manipulate columns of data.

### 5. What is xargs?

It converts standard input into command-line arguments for another command.

### 6. What is tee?

It writes output to both the terminal and a file simultaneously.

### 7. What is a Here Document?

A way to provide multi-line input directly within a shell script.

### 8. What is getopts?

A built-in utility for parsing command-line options in shell scripts.

### 9. When have you used sed in DevOps?

To update Docker image tags or configuration values automatically during deployments.

### 10. When have you used awk?

To extract fields from commands like kubectl get pods, ps, or log files.
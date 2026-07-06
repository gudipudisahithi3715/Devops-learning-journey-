# Day 04 - Linux Process Management (Advanced) & Service Management

When multiple processes compete for CPU time, Linux uses process priority to decide which process should receive CPU time first.

Linux uses a Nice Value to determine the priority of a process.

### Nice Value Range

-20 ---------------------------- 19
Highest Priority          Lowest Priority

Important Rule

Lower Nice Value → Higher Priority

Higher Nice Value → Lower Priority

A process with a higher nice value is considered "nice" because it allows other processes to use the CPU first.

# Commands
Start a Process with a Nice Value
nice -n 10 command

### Example:

nice -n 15 ./backup.sh

Starts the backup process with lower priority.

Change Priority of a Running Process
sudo renice -n 5 -p PID

### Example

sudo renice -n 5 -p 2456

Changes the priority of the running process.

# Real-Time DevOps Example

Suppose a backup script starts while customers are actively using your application.

Instead of giving the backup equal CPU priority:

nice -n 15 ./backup.sh

Linux gives higher priority to production applications like:

Nginx
MySQL
Jenkins
Kubernetes

This improves application performance.

#  Zombie Process
### Theory

A Zombie Process is a process that has completed execution, but its entry still exists in the process table because the parent process has not yet collected the child's exit status.

The process is already dead.

Only its process table entry remains.

### Characteristics
Child process has finished execution.
Parent process is still alive.
Child's exit status has not been collected.
Does not consume CPU.
Occupies a process table entry.
Identify Zombie Process
### ps -el

Look for:

STAT = Z

Z indicates a Zombie Process.

Can We Kill a Zombie Process?

❌ No.

The process is already dead.

Only the process table entry exists.

To remove it:

Restart or terminate the parent process.
Parent should collect the child's exit status.
Why Are Zombie Processes Dangerous?

One Zombie Process is usually harmless.

### However, if many Zombie Processes accumulate:

Process table becomes full.
New processes cannot be created.
Applications may hang.
System performance degrades.
#  Real-Time DevOps Example

Suppose a Java application creates many child processes.

The parent process never collects the exit status.

Eventually:

Thousands of Zombie Processes appear.
Application becomes unstable.
Users experience downtime.
# Orphan Process
### Theory

An Orphan Process is a child process whose parent process terminates before the child finishes execution.

Linux automatically assigns the orphan process to init/systemd (PID 1).

### Characteristics
Parent process has terminated.
Child process is still running.
Adopted by init/systemd.
Continues execution normally.
### Identify Orphan Process
ps -ef

Look for:

PPID = 1

This indicates the process has been adopted by init or systemd.

Are Orphan Processes Dangerous?

No.

Linux automatically manages them.

They continue running normally under init/systemd.

### Zombie vs Orphan Process
Zombie Process	Orphan Process
Child process is dead	Child process is running
Parent is alive	Parent is terminated
Process table entry remains	Child adopted by init/systemd
STAT = Z	PPID = 1
Can fill process table	Generally harmless
# Real-Time DevOps Example

Suppose Jenkins starts a child process to build a project.

If Jenkins unexpectedly crashes:

The build process continues.
Linux assigns it to systemd (PID 1).
The build can complete successfully.
# Interview Questions & Answers (Part 1)
### Q1. Why do we need process priority?

Answer:

Linux uses process priority to determine which process receives CPU time first when multiple processes compete for CPU resources.

### Q2. What is the range of Nice Values?

Answer:

-20 to 19

-20 → Highest Priority

19 → Lowest Priority

### Q3. Which has higher priority: -10 or 10?

Answer:

-10 has higher priority because lower nice values receive more CPU time.

### Q4. Difference between nice and renice?

Answer:

nice starts a new process with a specified priority.
renice changes the priority of an already running process.
### Q5. Why run backup scripts with Nice Value 15?

Answer:

Backup jobs are not time-critical.

Assigning them a higher nice value gives production applications higher CPU priority.

### Q6. What is a Zombie Process?

Answer:

A Zombie Process is a completed child process whose process table entry remains because the parent process has not collected its exit status.

### Q7. Does a Zombie Process consume CPU?

Answer:

No.

It only occupies a process table entry.

### Q8. How do you identify a Zombie Process?

Answer:

ps -el

Look for:

STAT = Z
### Q9. Can you kill a Zombie Process?

Answer:

No.

The process has already terminated.

Restart or terminate the parent process so it can remove the zombie entry.

### Q10. Why are multiple Zombie Processes dangerous?

Answer:

They occupy entries in the process table.

If too many accumulate, the system may fail to create new processes, leading to application failures.

### Q11. What is an Orphan Process?

Answer:

An Orphan Process is a child process whose parent terminates before the child finishes execution.

Linux assigns it to init/systemd (PID 1).

### Q12. Who adopts an Orphan Process?

Answer:

init or systemd (PID 1).

### Q13. How do you identify an Orphan Process?

Answer:

ps -ef

If:

PPID = 1

the process has been adopted by init/systemd.

### Q14. Are Orphan Processes dangerous?

Answer:

No.

Linux automatically manages them by assigning them to init/systemd.

### Q15. Difference between Zombie and Orphan Processes?

Answer:

A Zombie Process has completed execution but still has an entry in the process table because its parent hasn't collected its exit status.

An Orphan Process is still running after its parent terminates and is automatically adopted by init/systemd.

# Linux Service Management (systemctl)
📖 Theory

A service is a program that runs continuously in the background to provide a specific functionality.

Examples:

Jenkins
Docker
Nginx
MySQL
SSH
Kubernetes (kubelet)

Unlike normal programs, services usually start automatically during system boot and continue running until they are stopped.

As a DevOps Engineer, you'll frequently manage services during deployments, troubleshooting, and server administration.

# What is systemctl?

systemctl is a command-line utility used to manage services on Linux systems that use systemd.

Using systemctl, you can:

Start services
Stop services
Restart services
Reload configuration
Check service status
Enable services during boot
Disable automatic startup
### Common Commands
### Check Service Status
sudo systemctl status nginx

Displays:

Running status
PID
Recent logs
Service information
### Start a Service
sudo systemctl start nginx

Starts the service immediately.

### Stop a Service
sudo systemctl stop nginx

Stops the running service.

### Restart a Service
sudo systemctl restart nginx

Stops and starts the service again.

Used after:

Software updates
Configuration changes
Deployments
Reload a Service
sudo systemctl reload nginx

### Reloads the configuration without stopping the service.

Not every service supports reload.

# Restart vs Reload
Restart	Reload
Stops and starts the service	Reloads configuration without stopping the service
May cause temporary downtime	No downtime (if supported)
Used after major changes	Used after configuration changes
# Enable a Service
sudo systemctl enable docker

Configures Docker to start automatically after every system reboot.

#  Disable a Service
sudo systemctl disable docker

Prevents Docker from starting automatically during boot.

It does not stop the currently running service.

#  Difference Between start and enable
Start	Enable
Starts the service immediately	Starts automatically after every reboot
Current boot session only	Future boot sessions
#  Difference Between stop and disable
Stop	Disable
Stops the service immediately	Prevents automatic startup after reboot
Current session	Future boot sessions
#  Check if Service is Enabled
sudo systemctl is-enabled docker

Output:

enabled

or

disabled
#  Check if Service is Running
sudo systemctl is-active docker

Output:

active

or

inactive
#  Real-Time DevOps Example

A production server is restarted.

After reboot:

Jenkins is down.
Docker is down.

Reason:

The services were started using:

sudo systemctl start docker

but were not enabled.

Correct approach:

sudo systemctl enable docker
sudo systemctl start docker

This ensures the service starts now and automatically after future reboots.

#  Linux Logs
### Theory

Logs are records of events occurring on a Linux system.

They help administrators troubleshoot:

Application failures
Service crashes
Login attempts
Authentication failures
Kernel messages
System events

Logs act as the history of the operating system.

#  Log Location

Most Linux logs are stored in:

/var/log/
#  Common Log Files
Log File	Purpose
/var/log/syslog	General system logs (Ubuntu)
/var/log/messages	General system logs (RHEL/CentOS)
/var/log/auth.log	Authentication & SSH logs
/var/log/kern.log	Kernel logs
/var/log/dmesg	Boot & hardware logs
#  What is journalctl?

Modern Linux systems use systemd, which stores logs in a centralized journal.

The journalctl command is used to view these logs.

#  Common Commands
### View All Logs
journalctl
### View Logs of a Service
sudo journalctl -u nginx

Example:

sudo journalctl -u docker
sudo journalctl -u jenkins
### Show Last 20 Logs
journalctl -n 20
### Live Log Monitoring
journalctl -f

### Shows new log entries in real time.

Logs Since a Specific Time
journalctl --since "1 hour ago"

Example:

journalctl --since "2026-07-06 10:00:00"
#  Log Rotation
### Theory

Applications continuously generate logs.

Without log rotation:

Disk becomes full.
Applications fail.
Server performance degrades.

Linux uses logrotate to manage log files automatically.

### Main Configuration
/etc/logrotate.conf

### Application-specific configuration:

/etc/logrotate.d/
### What Does Log Rotation Do?
Rotates old log files.
Compresses old logs.
Creates a new log file.
Deletes old logs after the configured retention period.
#  Reading Log Files
### head
head /var/log/syslog

Shows the first 10 lines.

### tail
tail /var/log/syslog

Shows the last 10 lines.

### tail -f
tail -f /var/log/nginx/error.log

Continuously monitors new log entries.

Very useful during deployments and troubleshooting.

### less
less /var/log/syslog

Allows scrolling through large log files page by page.

### grep
grep ERROR app.log

Searches for specific text within log files.

Useful for quickly finding errors and exceptions.

#  Real-Time DevOps Example

A client reports:

"The website is not working."

Troubleshooting steps:

sudo systemctl status nginx

↓

sudo journalctl -u nginx

↓

tail -f /var/log/nginx/error.log

↓

grep ERROR /var/log/nginx/error.log

This systematic approach helps identify the root cause efficiently.

#  Interview Questions & Answers (Part 2)
### Q16. What is a Linux service?

Answer:

A service is a program that runs continuously in the background to provide specific functionality, such as web serving, database management, or CI/CD automation.

### Q17. What is systemctl?

Answer:

systemctl is a command-line utility used to manage services on Linux systems that use the systemd init system.

### Q18. Difference between start and enable?

Answer:

start starts a service immediately for the current session, while enable configures it to start automatically after every system reboot.

### Q19. Difference between stop and disable?

Answer:

stop stops the currently running service, whereas disable prevents the service from starting automatically during future boots.

### Q20. Difference between restart and reload?

Answer:

restart stops and starts the service again, which may cause temporary downtime. reload applies configuration changes without stopping the service, if supported.

### Q21. Which command checks the status of a service?

Answer:

sudo systemctl status service_name
### Q22. Which command checks whether a service is running?

Answer:

sudo systemctl is-active service_name
### Q23. Which command checks whether a service starts automatically after reboot?

Answer:

sudo systemctl is-enabled service_name
### Q24. Where are Linux log files stored?

Answer:

Most Linux log files are stored in:

/var/log/
### Q25. What is journalctl?

Answer:

journalctl is used to view logs collected by the systemd journal, allowing administrators to troubleshoot services and system events.

### Q26. Why is Log Rotation important?

Answer:

Log Rotation prevents log files from growing indefinitely by rotating, compressing, and removing old logs, ensuring efficient disk space management.

### Q27. What is the purpose of tail -f?

Answer:

tail -f continuously displays new log entries as they are written, making it useful for live monitoring during deployments and troubleshooting.

### Q28. Why is grep useful for DevOps engineers?

Answer:

grep quickly searches log files for specific keywords, such as ERROR or Exception, helping identify issues without manually reading large files.

# Linux Storage Management
### Theory

Storage Management is one of the most important responsibilities of a DevOps Engineer.

Applications such as:

Jenkins
Docker
Kubernetes
MySQL
Nginx

continuously store data, logs, images, and artifacts.

If disk space becomes full, applications may stop working or deployments may fail.

Storage management helps monitor disk usage, identify space-consuming files, and manage disks efficiently.

#  df (Disk Free)
### Theory

The df command displays the disk space usage of all mounted file systems.

It shows:

Total Size
Used Space
Available Space
Usage Percentage
Mount Point
###  Commands
Check Disk Space
df
Human Readable Format
df -h
#  Real-Time DevOps Example

A Jenkins deployment fails with:

No space left on device

First command to execute:

df -h

Check whether any filesystem has reached 100% usage.

#  du (Disk Usage)
### Theory

The du command displays the disk usage of a file or directory.

Unlike df, which shows disk usage of the entire filesystem, du shows how much space a specific directory or file occupies.

###  Commands
du

Human Readable

du -h

Directory Summary

du -sh /var/log
#  Difference Between df and du
df	du
Shows disk usage of the filesystem	Shows disk usage of files/directories
Used to check available disk space	Used to identify which directory consumes space
#  Real-Time DevOps Example

Disk usage is:

95%

Find the largest directory:

du -sh /var/*

Output:

/var/log    35G

Now you know log files are consuming disk space.

#  lsblk
### Theory

lsblk lists all block storage devices connected to the system.

It displays:

Disks
Partitions
Mount Points
###  Command
lsblk

Example Output

sda
├── sda1
├── sda2
sdb
└── sdb1
#  Real-Time DevOps Example

A new AWS EBS volume is attached to an EC2 instance.

Verify that Linux detects it:

lsblk
#  fdisk
### Theory

fdisk is used to create, modify, and delete disk partitions.

It is generally used after attaching a new disk to a Linux server.

### Command
sudo fdisk /dev/sdb
 Real-Time DevOps Example

Company adds a new 100 GB disk.

Steps:

Verify the disk using lsblk
Partition it using fdisk
Create a filesystem
Mount the partition
Update /etc/fstab
#  mount
### Theory

Linux cannot use a storage device until it is mounted.

Mounting connects a filesystem to a directory.

### Command
sudo mount /dev/sdb1 /data

Now the disk is accessible through:

/data
#  umount
### Theory

Unmounting disconnects a mounted filesystem.

Used before:

Removing disks
Maintenance
Resizing partitions
### Command
sudo umount /data
#  /etc/fstab
### Theory

/etc/fstab stores information about filesystems that should be mounted automatically during system boot.

Without adding an entry to /etc/fstab, the disk must be mounted manually after every reboot.

Example Entry
UUID=xxxx-xxxx   /data   ext4   defaults   0 2
#  Real-Time DevOps Example

An EBS volume is mounted successfully.

After reboot:

/data

is missing.

Reason:

The administrator forgot to add the filesystem entry to:

/etc/fstab
#  Inodes
### Theory

An inode stores metadata about a file.

It contains:

Owner
Permissions
File Size
Timestamps
Disk Block Locations

It does not store:

File Name
File Data

Every file has its own inode.

#  Disk Full vs Inode Full
Disk Full
Disk Usage = 100%

Need to delete large files.

Inode Full
Disk Usage = 25%
Inodes = 100%

Disk space is available.

However, no new files can be created because all inodes are used.

Usually caused by millions of very small files.

Check Inode Usage
df -i
#  Real-Time DevOps Example

An application creates millions of tiny log files.

Result:

Disk Usage = 30%
Inodes = 100%

Users cannot create new files.

Solution:

Delete unnecessary small files to free inodes.

#  Quick Revision
Command	Purpose
df -h	Check disk usage
du -sh	Check directory size
lsblk	List disks and partitions
fdisk	Manage partitions
mount	Mount a filesystem
umount	Unmount a filesystem
/etc/fstab	Automatic mounting after reboot
df -i	Check inode usage
#  Real-Time DevOps Scenarios
### Scenario 1: Jenkins Build Failed

Error:

No space left on device

Solution:

df -h
du -sh /var/*

Identify the directory consuming disk space and clean it.

### Scenario 2: Docker Consuming Disk Space

Check disk usage:

df -h

Identify Docker storage:

du -sh /var/lib/docker

Clean unused Docker resources if appropriate.

### Scenario 3: New AWS EBS Volume Attached

Steps:

lsblk
sudo fdisk /dev/sdb
sudo mount /dev/sdb1 /data

Update:

/etc/fstab

to ensure automatic mounting after reboot.

#  Interview Questions & Answers (Part 3)
### Q29. What is df?

Answer:

df displays disk space usage of mounted filesystems, including total, used, available space, and usage percentage.

### Q30. Why do we use df -h?

Answer:

The -h option displays disk sizes in a human-readable format such as MB, GB, and TB.

### Q31. What is the difference between df and du?

Answer:

df shows disk usage of the entire filesystem, while du shows the space used by specific files or directories.

### Q32. What does lsblk display?

Answer:

lsblk displays block storage devices, their partitions, and mount points.

### Q33. What is fdisk used for?

Answer:

fdisk is used to create, delete, and manage disk partitions.

### Q34. Why is mounting required?

Answer:

A filesystem must be mounted before Linux can access and use the storage device.

### Q35. What is the purpose of /etc/fstab?

Answer:

It stores filesystem mount information so that disks are mounted automatically during system boot.

### Q36. What happens if /etc/fstab is not updated?

Answer:

The disk will not be mounted automatically after a reboot and must be mounted manually.

### Q37. What is an inode?

Answer:

An inode is a data structure that stores metadata about a file, such as ownership, permissions, timestamps, and disk block locations.

### Q38. Does an inode store the file name?

Answer:

No. An inode stores metadata only. The filename is stored separately in the directory entry.

### Q39. Can a system run out of inodes while disk space is still available?

Answer:

Yes. If millions of small files consume all available inodes, no new files can be created even though disk space remains.

### Q40. Which command checks inode usage?

Answer:

df -i


# 1. What is a Process?

A Process is a program that is currently executing. A program stored on the disk becomes a process only when the Linux kernel loads it into memory and starts executing it.

Example

backup.sh  (Program)
      │
      ▼
./backup.sh
      │
      ▼
Running Process

💡 Key Point

Program = Stored on Disk
Process = Running in Memory
# 2. Why Do We Need Processes?

Processes allow Linux to:

Execute multiple applications simultaneously.
Isolate applications from each other.
Allocate CPU and memory efficiently.
Manage and schedule running applications.
Prevent one application's failure from affecting others.

### 💡 Real-Time Example

A production server may run:

Jenkins
Docker
Nginx
Prometheus
Grafana
Java Application

Each runs as a separate process. If one process crashes, the others continue running.

# 3. Program vs Process
Program	Process
Static file stored on disk	Running instance of a program
Passive	Active
Does not use CPU	Uses CPU and Memory
Exists until executed	Exists only while running
# 4. One Program Can Create Multiple Processes

A single program can create multiple processes depending on its functionality.

Example – Google Chrome
Chrome
│
├── Browser Process
├── Tab 1 Process
├── Tab 2 Process
├── GPU Process
└── Extension Process

Each browser tab runs in a separate process. If one tab crashes, the others continue to work.

# 5. Parent Process & Child Process

A Parent Process is a process that creates another process.

A Child Process is the process created by the parent process.

Example
Bash Shell
│
├── ls
├── pwd
├── cat
└── python

Here:

Bash = Parent Process
ls, pwd, cat, python = Child Processes
# 6. Process ID (PID)

Every running process in Linux is assigned a unique Process ID (PID).

Think of PID as the Aadhaar number of a process.

Example
Docker   → PID 1500
Jenkins  → PID 2050
Nginx    → PID 3010
# 7. Parent Process ID (PPID)

PPID stands for Parent Process ID.

It stores the PID of the process that created the current process.

Example
Bash
PID = 2000

      │
      ├── ls
      │    PID = 2100
      │    PPID = 2000
      │
      └── cat
           PID = 2101
           PPID = 2000

### 💡 Key Point

Every process has its own unique PID.
Child processes created by the same parent share the same PPID.
# 8. Linux Process Commands
### 8.1 ps (Process Status)

The ps command displays information about processes running in the current terminal session.

Syntax
ps
Output Columns
PID – Process ID
TTY – Terminal associated with the process
TIME – CPU time used by the process
CMD – Command or program that started the process
### 8.2 ps -e

Displays all running processes in the system.

Syntax
### ps -e

Use Case

Used when you want to see every running process, not just those in the current terminal.

### 8.3 ps -ef

Displays all running processes with full details.

Syntax
ps -ef
Additional Information Displayed
User (UID)
PID
PPID
Start Time
Terminal
CPU Time
Full Command

💡 Real-Time DevOps Example

Check whether Jenkins is running:

ps -ef | grep jenkins
### 8.4 ps aux

Displays all processes along with CPU and Memory usage.

Syntax
ps aux

Displays:

User
PID
CPU Usage (%CPU)
Memory Usage (%MEM)
Full Command

### 💡 Real-Time DevOps Example

If a production server becomes slow, use:

ps aux

to identify which process is consuming the most CPU or memory.

# 9. Real-Time DevOps Scenarios
### Scenario 1 – Jenkins Service Not Responding

Check whether the Jenkins process is running.

ps -ef | grep jenkins
### Scenario 2 – Docker Daemon Troubleshooting
ps -ef | grep dockerd

Verify whether the Docker daemon is active.

### Scenario 3 – Java Application Crash
ps -ef | grep java

Check if the Java application process exists.

### Scenario 4 – High CPU Usage
ps aux

Identify the process consuming the highest CPU or memory.

# 10. Key Takeaways
A Process is a program in execution.
The Linux Kernel creates and manages processes.
One program can create multiple child processes.
Every process has a unique PID.
PPID identifies the parent process.
ps = Process Status.
ps shows processes running in the current terminal.
ps -e shows all running processes.
ps -ef shows all processes with detailed information.
ps aux displays CPU and memory usage of all running processes.
# 11. Interview Questions & Answers
### Q1. What is a Process?

Answer:

A process is a program that is currently executing. When a program is loaded into memory and starts running, it becomes a process.

### Q2. What is the difference between a Program and a Process?

Answer:

A program is a static file stored on disk, whereas a process is the running instance of that program in memory.

### Q3. Who creates and manages processes in Linux?

Answer:

The Linux Kernel creates, schedules, manages, and terminates processes.

### Q4. Can one program create multiple processes? Explain with an example.

Answer:

Yes. A single program can create multiple processes. For example, Google Chrome creates separate processes for browser tabs, GPU, extensions, and network services. This improves stability because if one process crashes, the others continue running.

### Q5. What is a Parent Process?

Answer:

A Parent Process is a process that creates one or more child processes.

### Q6. What is a Child Process?

Answer:

A Child Process is a process created by another process (its parent) to perform a specific task.

### Q7. What is PID?

Answer:

PID (Process ID) is a unique numeric identifier assigned to every running process in Linux.

### Q8. What is PPID?

Answer:

PPID (Parent Process ID) stores the PID of the process that created the current process.

### Q9. What is the difference between PID and PPID?

Answer:

PID uniquely identifies a process, while PPID identifies the parent process that created it.

### Q10. What does ps stand for?

Answer:

ps stands for Process Status.

### Q11. What is the difference between ps and ps -e?

Answer:

ps displays processes running in the current terminal session, whereas ps -e displays all running processes in the system.

### Q12. Why do we use ps -ef?

Answer:

ps -ef displays all running processes with full details such as UID, PID, PPID, start time, and the complete command. It is commonly used for troubleshooting.

### Q13. When would you use ps aux?

Answer:

ps aux is used to monitor all running processes and identify high CPU or memory usage, especially when troubleshooting performance issues.

### Q14. In a production environment, when would you use ps -ef?

Answer:

I would use ps -ef while troubleshooting services such as Jenkins, Docker, Nginx, or Java applications to verify whether the process is running and gather details like PID, PPID, and the full command before taking further action.

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

#  High CPU & Memory Consuming Processes
### What is %CPU?
%CPU shows how much CPU a process is currently using.
A high %CPU value indicates that the process is heavily utilizing the processor.

Example:

### top

Output:

PID   USER      %CPU   %MEM   COMMAND
2050  jenkins   92.0    35.0   java

Here, the Java process owned by Jenkins is consuming 92% CPU.

# What is %MEM?
%MEM shows how much RAM a process is using.
A high %MEM value means the process is consuming a significant amount of memory.

Example:

PID   USER    %CPU   %MEM   COMMAND
3120  mysql    2.0    80.0   mysqld

Here, MySQL is using 80% of the system's RAM.

### USER Column

Displays the user who started the process.

Examples:

root
ubuntu
jenkins
mysql
### COMMAND Column

Displays the application or command that started the process.

### Examples:

java
nginx
mysqld
docker
### Real-Time DevOps Example

A client reports that the application is slow.

Run:

top

Check:

Which process has high %CPU
Which process has high %MEM
Identify the application causing the issue

Important: Never kill a process immediately. First verify whether the resource usage is expected and investigate the root cause.

# Process Termination
### kill

Used to send a signal to a specific process using its PID.

### Syntax:

kill PID

Example:

kill 2050
# Signals

A signal is a message sent to a process instructing it to perform an action.

### SIGTERM (15)

Gracefully terminates a process.

kill PID

or

kill -15 PID

Allows the application to:

Save data
Close files
Release resources
Exit cleanly
### SIGKILL (9)

Forcefully terminates a process.

kill -9 PID

The process is stopped immediately without cleanup.

### Best Practice

Always use:

SIGTERM
      ↓
If not responding
      ↓
SIGKILL

Never use kill -9 as the first option in production.

# kill vs killall vs pkill
### kill

Stops a specific process using its PID.

kill 2050
### killall

Stops all processes with the same process name.

killall nginx
### pkill

Stops processes using a process name or pattern.

pkill java

Search the full command line:

### pkill -f spring
Comparison
Command	Purpose	Requires PID
kill	Stop one specific process	Yes
killall	Stop all processes with the same name	No
pkill	Stop processes by name or pattern	No
# Foreground & Background Processes
### Foreground Process
Runs directly in the terminal.
Occupies the terminal until completion.
You cannot execute other commands in the same terminal.

### Example:

ping google.com
### Background Process
Runs without occupying the terminal.
Allows you to continue using the terminal.

Start a background process:

./backup.sh &

The & operator runs the process in the background.

### Foreground vs Background
Foreground	Background
Occupies terminal	Does not occupy terminal
User waits	User can continue working
Suitable for short tasks	Suitable for long-running tasks
# jobs Command

Displays background jobs started from the current shell.

### jobs

Example:

[1]+ Running    ./backup.sh &
jobs vs ps
jobs	ps
Shows background jobs of the current shell	Shows running processes
Uses Job Number	Uses PID
# Ctrl + Z, bg and fg
### Ctrl + Z

Suspends (pauses) the currently running foreground process.

The process is not terminated.

### bg

Resumes a suspended process in the background.

bg
### fg

Brings a background process back to the foreground.

fg
### Flow
Foreground Process
        │
   Ctrl + Z
        │
        ▼
Stopped (Paused)
        │
      bg
        │
        ▼
Background Process
        │
      fg
        │
        ▼
Foreground Again
# nohup
### What is nohup?

nohup stands for No Hang Up.

It keeps a process running even after:

User logout
SSH session disconnect
### Syntax
nohup command &

Example:

nohup ./backup.sh &
Default Output

By default, output is stored in:

nohup.out
Redirect Output
nohup ./backup.sh > backup.log 2>&1 &

### Explanation:

nohup → Keeps the process running after logout.
> → Redirects standard output.
backup.log → Stores logs.
2>&1 → Redirects standard error to the same file.
& → Runs the process in the background.
Difference Between & and nohup
&	nohup
Runs in the background	Runs in the background and survives logout/SSH disconnect
May stop when the terminal closes	Continues running after logout

### Note: nohup does not survive a server shutdown. It only keeps the process running after logout or SSH session disconnection.

# Real-Time DevOps Examples
### Scenario 1: Investigating a Slow Server

Run:

top

Check:

%CPU
%MEM
Load Average
Identify the resource-intensive process before taking any action.
### Scenario 2: Gracefully Stop a Service
kill PID

If the process does not respond:

kill -9 PID
Scenario 3: Long-Running Backup

Instead of:

./backup.sh

Use:

nohup ./backup.sh > backup.log 2>&1 &

The backup continues even after logout.

# Top Interview Questions
### Q1. What is the difference between %CPU and %MEM?

Answer:

%CPU shows the CPU utilization of a process.
%MEM shows the RAM utilization of a process.
### Q2. Why shouldn't you use kill -9 first?

Answer:

kill -9 forcefully terminates a process without allowing it to clean up resources or save its state. Always use SIGTERM first for a graceful shutdown.

### Q3. What is the difference between kill, killall, and pkill?

Answer:

kill terminates a specific process using its PID.
killall terminates all processes with the same name.
pkill terminates processes by matching a name or pattern.
### Q4. What is the difference between a foreground and a background process?

Answer:

A foreground process occupies the terminal until it completes, while a background process runs without occupying the terminal, allowing the user to execute other commands.

### Q5. What is the purpose of the jobs command?

Answer:

The jobs command displays background jobs started from the current shell.

### Q6. What does Ctrl + Z do?

Answer:

It suspends (pauses) the currently running foreground process without terminating it.

### Q7. What is the difference between bg and fg?

Answer:

bg resumes a suspended process in the background.
fg brings a background process back to the foreground.
### Q8. What is nohup?

Answer:

nohup allows a process to continue running even after the user logs out or the SSH session disconnects.

### Q9. What is the difference between & and nohup?

Answer:

& runs a process in the background.
nohup runs a process in the background and ensures it continues running after logout or SSH disconnection.
### Q10. Explain this command:
nohup java -jar app.jar > app.log 2>&1 &

Answer:

nohup → Keeps the application running after logout.
java -jar app.jar → Starts the Java application.
> → Redirects standard output.
app.log → Stores application logs.
2>&1 → Redirects standard error to the same log file.
& → Runs the application in the background.
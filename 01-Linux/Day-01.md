# Day 1 - Linux Fundamentals

# 1. What is an Operating System (OS)?

An **Operating System (OS)** is system software that acts as a bridge between the user/applications and the computer hardware. It manages hardware resources such as the CPU, memory, storage, and input/output devices, and provides an environment where applications can run efficiently.

Without an operating system, users and applications cannot effectively interact with the computer hardware.

---

# 2. Why can't applications communicate directly with hardware?

Applications cannot communicate directly with hardware because they do not know how to interact with different hardware devices such as CPUs, memory, storage, printers, or network cards.

The operating system provides a standard interface between applications and hardware. It receives requests from applications, communicates with the hardware, and returns the results to the application. This makes applications independent of hardware-specific details.

### Flow

User/Application → Operating System → Hardware

---

# 3. What is Linux?

Linux is an **open-source operating system based on the Linux kernel**.

Technically, the **Linux kernel** is the core component of the operating system. When the kernel is combined with system utilities, libraries, package managers, and other software, it forms a complete operating system called a **Linux distribution**.

Linux manages hardware resources and provides an environment for applications to run efficiently.

---

# 4. Why is Linux preferred in DevOps?

Linux is widely used in DevOps because:

* It is open source.
* It is stable and reliable.
* It provides strong security through its permissions model.
* It is efficient and uses system resources effectively.
* It is automation-friendly through Bash scripting and the command line.
* Most production servers run Linux.
* Popular DevOps tools such as Docker, Kubernetes, Jenkins, Terraform, and Ansible work very well on Linux.

---

# 5. Common Linux Distributions

## Ubuntu

* Beginner-friendly and widely used for servers, cloud, and DevOps.

## Debian

* Known for its stability and is the base distribution for Ubuntu.

## Fedora

* Includes newer technologies and is popular among developers.

## Red Hat Enterprise Linux (RHEL)

* Enterprise Linux distribution widely used by organizations with commercial support.

## Rocky Linux

* Free, enterprise-grade Linux distribution that is commonly used as an alternative to RHEL.

---

# Key Takeaways

* An Operating System acts as a bridge between applications and hardware.
* Applications communicate with hardware through the Operating System.
* Linux is based on the Linux kernel.
* A Linux distribution combines the Linux kernel with other software to create a complete operating system.
* Linux is the preferred operating system for most servers and DevOps environments because it is stable, secure, open source, and automation-friendly.
# Linux Architecture

Linux architecture describes how different components of the operating system work together to execute a command and communicate with the computer hardware.

## Components of Linux Architecture

### 1. User

The user interacts with the system by typing commands or using applications.

Example:

```bash
ls
mkdir DevOps
pwd
```

The user does not communicate directly with the hardware.

---

### 2. Terminal

The Terminal is the application or window where users type commands and view the output.

Examples of terminals include:

* Windows Terminal
* GNOME Terminal
* VS Code Integrated Terminal

The Terminal simply accepts user input and displays the output. It does not execute commands.

---

### 3. Shell

The Shell is a command-line interpreter.

Its responsibilities are:

* Read user commands.
* Interpret the commands.
* Locate the executable program (for example, `ls` is usually located in `/usr/bin/ls`).
* Make system calls to the Linux kernel.
* Receive the results from the kernel.
* Display the output in the terminal.

Common Linux shells:

* Bash
* Zsh
* Fish
* Korn Shell (Ksh)

Ubuntu uses **Bash** as the default shell.

---

### 4. System Call

A **System Call** is the communication mechanism between user-space applications and the Linux kernel.

Applications and the shell cannot directly access hardware. Instead, they request services from the kernel using system calls.

Examples of system calls include:

* Opening a file
* Reading a file
* Writing to a file
* Creating a process
* Allocating memory
* Accessing network resources

---

### 5. Kernel

The **Kernel** is the core of the Linux operating system.

It is loaded during system startup and remains in memory until the system shuts down.

The kernel is responsible for managing:

* CPU
* Memory (RAM)
* Storage devices
* File systems
* Device drivers
* Network communication
* Processes
* Security and permissions

The kernel is the only component that directly communicates with the hardware.

---

### 6. Hardware

Hardware includes the physical components of the computer such as:

* CPU
* RAM
* SSD/HDD
* Keyboard
* Mouse
* Network Card
* Monitor

Hardware performs the actual operations requested by the kernel.

---

# Flow of a Linux Command

Example:

```bash
ls
```

Execution flow:

1. The user types `ls` in the Terminal.
2. The Terminal sends the command to the Shell.
3. The Shell interprets the command.
4. The Shell locates the executable (`/usr/bin/ls`).
5. The Shell makes a system call to the Kernel.
6. The Kernel interacts with the file system and storage hardware.
7. The Hardware returns the requested information to the Kernel.
8. The Kernel sends the result back to the Shell.
9. The Shell displays the output in the Terminal.
10. The user sees the list of files and directories.

---

# Linux Architecture Diagram

```text
User
 │
 ▼
Terminal
 │
 ▼
Shell (Bash)
 │
 ▼
System Call
 │
 ▼
Kernel
 │
 ▼
Hardware
 │
 ▼
Kernel
 │
 ▼
Shell
 │
 ▼
Terminal
 │
 ▼
User
```

---

# Difference Between Terminal and Shell

| Terminal                                     | Shell                                               |
| -------------------------------------------- | --------------------------------------------------- |
| A window/application used to enter commands. | A command-line interpreter that processes commands. |
| Accepts input and displays output.           | Reads, interprets, and executes commands.           |
| Does not communicate with hardware.          | Communicates with the kernel through system calls.  |

---

# Key Points for Interviews

* Linux architecture consists of the User, Terminal, Shell, Kernel, and Hardware.
* The Terminal is only an interface for entering commands.
* The Shell interprets commands and communicates with the kernel.
* A System Call is a request made by an application or shell to the kernel.
* The Kernel is the core of the operating system and is responsible for managing all hardware resources.
* The Kernel is the only component that directly interacts with hardware.
* Every Linux command follows the flow: **User → Terminal → Shell → System Call → Kernel → Hardware → Kernel → Shell → Terminal → User**.
# Linux File System

## What is the Linux File System?

The Linux File System is a hierarchical structure used to organize and store files and directories. Unlike Windows, which uses multiple drives such as `C:\` and `D:\`, Linux has a **single directory tree** that starts from the **root directory (`/`)**.

Every file, directory, storage device, and mounted partition exists somewhere under this single root directory.

---

# Root Directory (`/`)

The **root directory (`/`)** is the top-most directory in Linux.

It acts as the starting point of the entire file system, and all other directories are located under it.

Example:

```text
/
├── home
├── etc
├── usr
├── var
├── tmp
├── boot
├── dev
├── opt
└── root
```

> **Note:** Do not confuse the **root directory (`/`)** with the **root user's home directory (`/root`)**.

---

# Important Linux Directories

## `/home`

The `/home` directory contains the personal home directories of all normal users.

Example:

```text
/home/sahigudipudi
```

This is where users store their documents, projects, downloads, and other personal files.

---

## `/root`

The `/root` directory is the home directory of the **root user (administrator)**.

Example:

```text
/              → Root directory
/root          → Root user's home directory
```

---

## `/etc`

The `/etc` directory stores **system-wide configuration files**.

Examples:

* SSH configuration
* Network configuration
* User account information
* Service configuration

As a DevOps Engineer, you'll frequently access this directory to modify application and server configurations.

---

## `/var`

The `/var` directory stores **variable data**, which changes frequently while the system is running.

Examples include:

* Log files
* Cache files
* Mail data
* Database files

One of the most important subdirectories is:

```text
/var/log
```

This is the first place to check when troubleshooting application or server issues.

---

## `/usr`

The `/usr` directory contains user applications, executable programs, libraries, and documentation.

Examples:

```text
/usr/bin
/usr/lib
/usr/share
```

Many Linux commands such as `ls`, `cp`, and `mv` are stored under `/usr/bin`.

---

## `/bin`

The `/bin` directory contains essential executable commands required for the operating system.

Examples:

```text
/bin/ls
/bin/cp
/bin/mv
/bin/cat
```

These commands are available even during system recovery.

---

## `/tmp`

The `/tmp` directory stores **temporary files** created by applications and the operating system.

The contents of this directory may be automatically removed after a reboot or periodically by the operating system.

It should not be used to store important data.

---

## `/opt`

The `/opt` directory is commonly used to install **third-party or optional software**.

Examples:

```text
/opt/sonarqube
/opt/tomcat
/opt/nexus
```

Many manually installed enterprise applications are placed here.

---

## `/dev`

The `/dev` directory contains **device files** that represent hardware devices.

Examples include:

* Hard disks
* USB devices
* Terminal devices
* CD/DVD drives

In Linux, many hardware devices are represented as files.

---

# Summary Table

| Directory | Purpose                                                 |
| --------- | ------------------------------------------------------- |
| `/`       | Root directory; starting point of the Linux file system |
| `/home`   | Home directories of normal users                        |
| `/root`   | Home directory of the root (administrator) user         |
| `/etc`    | System configuration files                              |
| `/var`    | Variable data such as logs, cache, and mail             |
| `/usr`    | User applications, libraries, and executables           |
| `/bin`    | Essential Linux commands and binaries                   |
| `/tmp`    | Temporary files                                         |
| `/opt`    | Third-party or optional software                        |
| `/dev`    | Device files representing hardware                      |

---

# Interview Questions

### 1. What is the Linux File System?

The Linux File System is a hierarchical structure that organizes files and directories under a single root directory (`/`).

---

### 2. What is the difference between `/` and `/root`?

* `/` is the root directory and the starting point of the Linux file system.
* `/root` is the home directory of the root (administrator) user.

---

### 3. Where are Linux configuration files stored?

Configuration files are generally stored in the `/etc` directory.

---

### 4. Where are log files stored?

Most system and application log files are stored in `/var/log`.

---

### 5. Where are third-party applications commonly installed?

Third-party applications are commonly installed in the `/opt` directory.

---

# Key Takeaways

* Linux uses a **single hierarchical file system** that begins at the root directory (`/`).
* Every file and directory exists somewhere under the root directory.
* Configuration files are stored in `/etc`.
* Log files are stored in `/var/log`.
* User files are stored in `/home`.
* The root user's home directory is `/root`.
* Temporary files are stored in `/tmp`.
* Third-party software is commonly installed in `/opt`.
* Hardware devices are represented as files under `/dev`.
# Absolute Path vs Relative Path

## What is a Path?

A **path** is the location of a file or directory in the Linux file system. It tells the operating system where a particular file or folder is located.

There are two types of paths in Linux:

1. Absolute Path
2. Relative Path

---

# Absolute Path

An **Absolute Path** is the complete path to a file or directory starting from the **root directory (`/`)**.

It always begins with `/` and is **independent of the current working directory**.

### Example

```text
/home/sahigudipudi/DevOps-2026/Devops-learning-journey
```

or

```bash
cd /home/sahigudipudi/DevOps-2026/Devops-learning-journey
```

No matter where you are in the Linux file system, this path always points to the same location.

### Real-life Analogy

Think of it as your **full home address**.

Example:

```text
House No. 10
ABC Colony
Hyderabad
Telangana
India
```

Anyone can reach your house using the complete address, regardless of where they start.

---

# Relative Path

A **Relative Path** specifies the location of a file or directory **relative to the current working directory (PWD)**.

It **does not start with `/`** and depends on where you are currently located.

### Example

Suppose your current directory is:

```text
/home/sahigudipudi
```

To move to your project, you can simply use:

```bash
cd DevOps-2026/Devops-learning-journey
```

Linux starts from your current location and follows the given path.

### Real-life Analogy

Imagine you are already standing in your living room and want to go to your bedroom.

Instead of saying your complete house address again, you simply say:

> "Go to the next room."

This is how a relative path works.

---

# Absolute Path vs Relative Path

| Absolute Path                                | Relative Path                                            |
| -------------------------------------------- | -------------------------------------------------------- |
| Starts from the root directory (`/`).        | Starts from the current working directory.               |
| Always begins with `/`.                      | Does not begin with `/`.                                 |
| Independent of the current location.         | Depends on the current location.                         |
| Always points to the same file or directory. | Changes depending on where the user currently is.        |
| Similar to giving a complete home address.   | Similar to giving directions from your current location. |

---

# Important Linux Commands

## `pwd` (Print Working Directory)

The `pwd` command displays the **current working directory**, showing where you are in the Linux file system.

Example:

```bash
pwd
```

Output:

```text
/home/sahigudipudi/DevOps-2026
```

---

## `cd` (Change Directory)

The `cd` command is used to move from one directory to another.

Examples:

Move to the root directory:

```bash
cd /
```

Move to your home directory:

```bash
cd ~
```

Move to the parent directory:

```bash
cd ..
```

Move to the current directory (rarely used directly but important in scripting):

```bash
cd .
```

Move using an absolute path:

```bash
cd /etc
```

Move using a relative path:

```bash
cd Documents
```

---

# Special Symbols in Linux

## `.` (Current Directory)

Represents the **current working directory**.

Example:

```bash
ls .
```

Lists the contents of the current directory.

---

## `..` (Parent Directory)

Represents the **parent directory** of the current directory.

Example:

Current directory:

```text
/home/sahigudipudi/DevOps-2026/01-Linux
```

Command:

```bash
cd ..
```

Result:

```text
/home/sahigudipudi/DevOps-2026
```

---

## `~` (Home Directory)

Represents the **home directory of the currently logged-in user**.

For example, if the current user is `sahigudipudi`:

```text
~
=
/home/sahigudipudi
```

For the root user:

```text
~
=
/root
```

Example:

```bash
cd ~
```

Moves directly to the current user's home directory.

---

# Interview Questions

### 1. What is the difference between an Absolute Path and a Relative Path?

An absolute path starts from the root directory (`/`) and always points to the same location. A relative path starts from the current working directory and depends on the user's current location.

---

### 2. What does the `pwd` command do?

The `pwd` command displays the current working directory.

---

### 3. What is the difference between `cd /` and `cd ~`?

* `cd /` moves to the root directory of the Linux file system.
* `cd ~` moves to the home directory of the currently logged-in user.

---

### 4. What do `.`, `..`, and `~` represent?

* `.` → Current directory
* `..` → Parent directory
* `~` → Home directory of the current user

---

# Key Takeaways

* A path specifies the location of a file or directory.
* An absolute path always starts from the root directory (`/`).
* A relative path starts from the current working directory.
* `pwd` displays the current working directory.
* `cd` changes the current directory.
* `.` represents the current directory.
* `..` represents the parent directory.
* `~` represents the current user's home directory.
# Linux File and Directory Management Commands

Linux provides a rich set of commands to create, navigate, copy, move, and remove files and directories. These commands are frequently used by Linux administrators and DevOps engineers while managing applications, configuration files, and server resources.

---

# `pwd` (Print Working Directory)

The `pwd` command displays the current working directory, helping users identify their current location in the Linux file system.

### Syntax

```bash
pwd
```

### Example

```bash
pwd
```

Output:

```text
/home/sahigudipudi/DevOps-2026/01-Linux
```

### Use Case

* Identify the current directory before performing file operations.
* Avoid accidental changes or deletion in the wrong directory.

---

# `cd` (Change Directory)

The `cd` command is used to navigate between directories.

### Syntax

```bash
cd <directory_name>
```

### Examples

Move to the root directory:

```bash
cd /
```

Move to the home directory:

```bash
cd ~
```

Move to the parent directory:

```bash
cd ..
```

Move to a directory using an absolute path:

```bash
cd /etc
```

Move to a directory using a relative path:

```bash
cd 01-Linux
```

### Use Case

* Navigate through the Linux file system.
* Access configuration files, project folders, or application directories.

---

# `ls` (List)

The `ls` command displays the contents of a directory.

### Syntax

```bash
ls
```

### Common Options

#### `ls`

Displays only visible files and directories.

#### `ls -a`

Displays all files, including hidden files whose names begin with a dot (`.`).

Example:

```text
.git
.bashrc
.profile
```

#### `ls -l`

Displays files and directories in **long listing format**, providing additional details such as:

* File type
* Permissions
* Owner
* Group
* File size
* Last modified date and time

The first character of the output indicates the file type:

* `d` → Directory
* `-` → Regular file
* `l` → Symbolic link

#### `ls -la`

Combines both `-l` and `-a` to display detailed information for all files, including hidden files.

### Use Case

* View directory contents.
* Verify permissions.
* Identify hidden configuration files.

---

# `mkdir` (Make Directory)

The `mkdir` command creates new directories.

### Syntax

```bash
mkdir directory_name
```

### Example

```bash
mkdir Practice
```

### Create Multiple Directories

```bash
mkdir Dev Test Prod
```

### Create Nested Directories

```bash
mkdir -p DevOps/Linux/Basics
```

The `-p` option automatically creates parent directories if they do not exist.

### Use Case

* Organize projects.
* Create application directory structures.
* Prepare folder hierarchies for deployments.

---

# `touch`

The `touch` command creates an empty file if it does not already exist. If the file exists, it updates the file's timestamp.

### Syntax

```bash
touch filename
```

### Example

```bash
touch notes.txt
```

### Create Multiple Files

```bash
touch file1.txt file2.txt README.md
```

### Use Case

* Create configuration files.
* Create log files.
* Quickly generate empty files for testing.

---

# `rmdir` (Remove Directory)

The `rmdir` command removes **only empty directories**.

### Syntax

```bash
rmdir directory_name
```

### Example

```bash
rmdir Practice
```

If the directory contains files or subdirectories, Linux returns an error because `rmdir` is designed to remove only empty directories.

### Use Case

* Safely remove empty directories.

---

# `rm` (Remove)

The `rm` command removes files and directories.

### Remove a File

```bash
rm notes.txt
```

### Remove a Directory and Its Contents

```bash
rm -r DevOps
```

The `-r` (recursive) option removes the directory along with all files and subdirectories inside it.

> **Warning:** Use `rm -r` carefully, as deleted files cannot be recovered easily.

### Use Case

* Delete unnecessary files.
* Clean up project directories.
* Remove old deployment folders.

---

# `cp` (Copy)

The `cp` command copies files or directories from one location to another.

### Syntax

```bash
cp source destination
```

### Copy a File

```bash
cp notes.txt backup.txt
```

This creates a copy while keeping the original file unchanged.

### Copy a File to Another Directory

```bash
cp notes.txt Practice/
```

### Copy a Directory

```bash
cp -r Project ProjectBackup
```

The `-r` (recursive) option copies the directory and all its contents.

### Use Case

* Create backup copies.
* Duplicate project files.
* Copy configuration files before modification.

---

# `mv` (Move)

The `mv` command moves files or directories from one location to another. It is also used to rename files and directories.

### Syntax

```bash
mv source destination
```

### Move a File

```bash
mv notes.txt Practice/
```

The original file is moved to the destination directory.

### Rename a File

```bash
mv notes.txt linux-notes.txt
```

This changes the file name without creating a copy.

> **Note:** If the destination file already exists, `mv` overwrites it by default (unless interactive options or aliases are configured).

### Use Case

* Organize project files.
* Rename files.
* Move logs, configuration files, or application artifacts.

---

# Comparison: `cp` vs `mv`

| `cp`                                   | `mv`                                             |
| -------------------------------------- | ------------------------------------------------ |
| Creates a copy of a file or directory. | Moves the original file or directory.            |
| Original remains unchanged.            | Original changes location.                       |
| Commonly used for backups.             | Commonly used for organizing and renaming files. |
| Requires `-r` to copy directories.     | Can move files or directories directly.          |

---

# Real DevOps Scenarios

### Backing Up Configuration Files

Before modifying an application configuration file:

```bash
cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bak
```

If the new configuration causes issues, the backup can be restored.

---

### Organizing Application Logs

```bash
mv app.log archive/
```

Moves old logs to an archive directory instead of deleting them.

---

### Creating Project Structure

```bash
mkdir -p DevOps/Linux/Basics
```

Creates the complete directory hierarchy in a single command.

---

# Interview Questions

### 1. What is the difference between `cp` and `mv`?

`cp` creates a duplicate of a file or directory while keeping the original intact. `mv` changes the location of the original file or directory and is also used to rename files.

---

### 2. Why do we use the `-r` option with `cp`?

The `-r` (recursive) option copies a directory along with all its files and subdirectories.

---

### 3. What is the difference between `rm` and `rmdir`?

* `rm` removes files and directories (with the `-r` option).
* `rmdir` removes only empty directories.

---

### 4. Why is `cp` commonly used before modifying configuration files?

It creates a backup so the original file can be restored if the new configuration causes issues.

---

### 5. What happens if the destination file already exists when using `mv`?

By default, `mv` overwrites the destination file unless interactive options or aliases are configured.

---

# Commands Practiced Today

```text
pwd
cd
cd /
cd ~
cd ..
ls
ls -a
ls -l
ls -la
mkdir
mkdir -p
touch
rmdir
rm
rm -r
cp
cp -r
mv
```

---

# Hands-on Activities Completed

* Navigated through the Linux file system.
* Practiced absolute and relative paths.
* Listed files and hidden files.
* Viewed file details using long listing format.
* Created directories and nested directories.
* Created empty files.
* Removed empty and non-empty directories.
* Copied files and directories.
* Moved and renamed files.
* Explored the practical use of Linux file management commands.

---

# Key Takeaways

* `pwd` displays the current working directory.
* `cd` is used to navigate between directories.
* `ls` lists directory contents, while `ls -a`, `ls -l`, and `ls -la` provide additional information.
* `mkdir` creates directories, and `mkdir -p` creates nested directories.
* `touch` creates empty files or updates timestamps.
* `rmdir` removes only empty directories.
* `rm` removes files, and `rm -r` removes directories recursively.
* `cp` creates copies, making it ideal for backups.
* `mv` moves or renames files and directories.
* Understanding these commands is fundamental for Linux administration and DevOps operations.
# Linux File Reading Commands

Linux provides several commands to read and inspect file contents. Each command is designed for a different purpose, such as displaying an entire file, viewing large files page by page, or monitoring log files in real time.

These commands are widely used by Linux administrators and DevOps engineers while working with configuration files, application logs, and system logs.

---

# `cat` (Concatenate)

The `cat` command stands for **Concatenate**. It was originally designed to combine the contents of multiple files and display them as a single output. Today, it is commonly used to display the contents of a file.

## Syntax

```bash
cat <filename>
```

## Example

```bash
cat devops.txt
```

Output:

```text
Linux
Git
Docker
Kubernetes
Jenkins
Terraform
Azure
```

## Use Cases

* Display the contents of a small file.
* Combine multiple files into one output.
* Quickly verify configuration files.

> **Note:** `cat` is not suitable for very large files because it displays the entire file at once, making it difficult to locate specific information.

---

# `less`

The `less` command displays a file **one screen at a time**, allowing users to scroll forward and backward without loading the entire file into the terminal.

## Syntax

```bash
less <filename>
```

## Example

```bash
less devops.txt
```

### Navigation

* **Down Arrow** → Scroll down
* **Up Arrow** → Scroll up
* **Space** → Next page
* **b** → Previous page
* **q** → Quit

## Use Cases

* Read large log files.
* Inspect configuration files.
* Navigate through files efficiently without flooding the terminal.

---

# `head`

The `head` command displays the **first 10 lines** of a file by default.

## Syntax

```bash
head <filename>
```

## Example

```bash
head devops.txt
```

Display the first 5 lines:

```bash
head -5 devops.txt
```

## Use Cases

* Verify the beginning of configuration files.
* Check file headers.
* Preview file contents.

---

# `tail`

The `tail` command displays the **last 10 lines** of a file by default.

## Syntax

```bash
tail <filename>
```

## Example

```bash
tail devops.txt
```

Display the last 3 lines:

```bash
tail -3 devops.txt
```

Display the last 20 lines:

```bash
tail -20 devops.txt
```

or

```bash
tail -n 20 devops.txt
```

## Use Cases

* Read the latest log entries.
* Check recent updates to a file.
* Troubleshoot applications.

---

# `tail -f` (Follow)

The `-f` option stands for **Follow**.

The `tail -f` command continuously monitors a file and displays new content as it is added.

## Syntax

```bash
tail -f <filename>
```

## Example

```bash
tail -f application.log
```

Whenever new log entries are written to the file, they appear automatically in the terminal.

To stop monitoring:

```text
Press Ctrl + C
```

## Use Cases

* Monitor application logs in real time.
* Observe server logs.
* Troubleshoot production issues.
* Watch container logs while applications are running.

---

# Comparison of File Reading Commands

| Command   | Purpose                                                             |
| --------- | ------------------------------------------------------------------- |
| `cat`     | Displays the entire file.                                           |
| `less`    | Displays a file one screen at a time for easy navigation.           |
| `head`    | Displays the first 10 lines of a file.                              |
| `tail`    | Displays the last 10 lines of a file.                               |
| `tail -f` | Continuously monitors a file and displays new content in real time. |

---

# Real DevOps Scenarios

## Viewing a Configuration File

```bash
cat /etc/nginx/nginx.conf
```

Used to quickly inspect a small configuration file.

---

## Reading a Large Log File

```bash
less /var/log/syslog
```

Allows easy navigation through a large log file.

---

## Checking the Beginning of a Configuration File

```bash
head /etc/nginx/nginx.conf
```

Useful for verifying headers or initial configuration settings.

---

## Viewing the Latest Application Logs

```bash
tail /var/log/app.log
```

Displays the most recent log entries.

---

## Monitoring Logs in Real Time

```bash
tail -f /var/log/app.log
```

Commonly used by DevOps engineers to monitor application logs during deployments, troubleshooting, or production incidents.

---

# Interview Questions

### 1. Why is `cat` not recommended for large files?

`cat` displays the entire file at once, which floods the terminal and makes it difficult to locate specific information. Commands like `less`, `head`, and `tail` are more suitable for large files.

---

### 2. When would you use `less` instead of `cat`?

`less` is used when working with large files because it displays one screen at a time and allows users to navigate forward and backward efficiently.

---

### 3. What is the difference between `head` and `tail`?

* `head` displays the first 10 lines of a file by default.
* `tail` displays the last 10 lines of a file by default.

---

### 4. What does `tail -f` do?

`tail -f` continuously monitors a file and displays new content as it is appended. It is widely used for monitoring application and server logs in real time.

---

### 5. Which command would you use to troubleshoot a running application?

I would use:

```bash
tail -f /var/log/app.log
```

because it allows me to monitor the latest log entries in real time without repeatedly reopening the log file.

---

# Commands Practiced Today

```text
cat
less
head
head -5
tail
tail -3
tail -20
tail -n 20
tail -f
```

---

# Hands-on Activities Completed

* Created a sample file.
* Displayed file contents using `cat`.
* Read a file page by page using `less`.
* Viewed the beginning of a file using `head`.
* Viewed the end of a file using `tail`.
* Displayed a custom number of lines using `head` and `tail`.
* Monitored a file in real time using `tail -f`.
* Stopped a running command using `Ctrl + C`.

---

# Key Takeaways

* `cat` is suitable for viewing small files.
* `less` is ideal for reading large files efficiently.
* `head` displays the beginning of a file.
* `tail` displays the end of a file.
* `tail -f` is one of the most important commands for DevOps engineers because it enables real-time log monitoring.
* Understanding log-reading commands is essential for troubleshooting applications and maintaining production systems.
# Linux File Permissions and Ownership

## Overview

Linux provides a security mechanism called **file permissions** to control who can access and modify files and directories. Every file and directory also has an **owner** and a **group**, which work together with permissions to secure the system.

Understanding permissions and ownership is essential for Linux administrators and DevOps engineers because they frequently troubleshoot permission-related issues in production environments.

---

# Why Do We Need File Permissions?

Without file permissions, any user could:

* View confidential data.
* Modify important configuration files.
* Delete application files.
* Execute unauthorized programs.

File permissions help protect files from unauthorized access and accidental or malicious modifications.

---

# Basic File Permissions

Linux provides three basic permissions.

| Permission | Symbol | Meaning                              |
| ---------- | ------ | ------------------------------------ |
| Read       | `r`    | View the contents of a file.         |
| Write      | `w`    | Modify the contents of a file.       |
| Execute    | `x`    | Run the file as a program or script. |

---

# Permission Categories

Permissions are assigned to three categories of users.

| Category     | Description                                                       |
| ------------ | ----------------------------------------------------------------- |
| Owner (`u`)  | The user who owns the file (usually the creator).                 |
| Group (`g`)  | A collection of users who share common permissions.               |
| Others (`o`) | Everyone else who is neither the owner nor a member of the group. |

---

# Viewing Permissions

Use the following command to view permissions:

```bash
ls -l
```

Example:

```text
-rwxr-xr--
```

Breakdown:

```text
-
rwx
r-x
r--
```

* First character (`-`) → Regular file
* `d` → Directory

Permission groups:

* Owner → `rwx`
* Group → `r-x`
* Others → `r--`

---

# Numeric Permission Values

Each permission has a numeric value.

| Permission    | Value |
| ------------- | ----: |
| Read (`r`)    |     4 |
| Write (`w`)   |     2 |
| Execute (`x`) |     1 |

Permissions are calculated by adding these values.

| Permission | Calculation | Value |
| ---------- | ----------- | ----: |
| `rwx`      | 4 + 2 + 1   |     7 |
| `rw-`      | 4 + 2       |     6 |
| `r-x`      | 4 + 1       |     5 |
| `r--`      | 4           |     4 |
| `-wx`      | 2 + 1       |     3 |
| `-w-`      | 2           |     2 |
| `--x`      | 1           |     1 |
| `---`      | 0           |     0 |

---

# Common Permission Values

## `755`

```text
Owner  -> rwx
Group  -> r-x
Others -> r-x
```

Used for executable scripts and directories where only the owner should modify the content.

---

## `644`

```text
Owner  -> rw-
Group  -> r--
Others -> r--
```

Commonly used for configuration files because only the owner can modify them while everyone else can read them.

---

## `777`

```text
Owner  -> rwx
Group  -> rwx
Others -> rwx
```

Grants full access to everyone.

**Not recommended** because any user can read, modify, or execute the file, creating security risks.

---

# chmod (Change Mode)

`chmod` is used to change file or directory permissions.

## Numeric Method

```bash
chmod 755 deploy.sh
```

```bash
chmod 644 config.conf
```

---

## Symbolic Method

Add execute permission:

```bash
chmod +x deploy.sh
```

Add execute permission to owner:

```bash
chmod u+x deploy.sh
```

Add write permission to group:

```bash
chmod g+w deploy.sh
```

Remove execute permission from others:

```bash
chmod o-x deploy.sh
```

Remove write permission:

```bash
chmod -w file.txt
```

---

# Difference Between Numeric and Symbolic Methods

| Numeric Method                   | Symbolic Method                       |
| -------------------------------- | ------------------------------------- |
| Sets all permissions explicitly. | Adds or removes specific permissions. |
| Example: `chmod 755 file.sh`     | Example: `chmod +x file.sh`           |

---

# File Ownership

Every file has:

* An Owner
* A Group

Ownership is different from permissions.

Permissions define **what actions** can be performed.

Ownership defines **who owns the file**.

---

# Viewing Ownership

```bash
ls -l
```

Example:

```text
-rwxr-xr-x 1 sahigudipudi sahigudipudi 250 Jun 30 10:00 deploy.sh
```

Here:

* First `sahigudipudi` → Owner
* Second `sahigudipudi` → Group

---

# chown (Change Owner)

Used to change the owner of a file or directory.

Syntax:

```bash
sudo chown <owner> <file>
```

Example:

```bash
sudo chown sahigudipudi deploy.sh
```

Change both owner and group:

```bash
sudo chown sahigudipudi:developers deploy.sh
```

---

# chgrp (Change Group)

Used to change only the group ownership.

Syntax:

```bash
sudo chgrp developers deploy.sh
```

---

# chmod vs chown vs chgrp

| Command | Purpose                                             |
| ------- | --------------------------------------------------- |
| `chmod` | Changes file permissions.                           |
| `chown` | Changes the owner of a file or directory.           |
| `chgrp` | Changes the group ownership of a file or directory. |

---

# Real DevOps Scenarios

## Deploying Shell Scripts

Deployment scripts are commonly assigned permission `755` so that:

* Owner can read, modify, and execute.
* Group and others can read and execute.

---

## Configuration Files

Configuration files are commonly assigned permission `644` because:

* Only the owner should modify them.
* Other users only need read access.

---

## Jenkins Pipeline Failure

A Jenkins pipeline may fail with:

```text
Permission denied
```

Possible causes:

* Missing execute permission.
* Incorrect file ownership.

Solutions:

```bash
chmod +x deploy.sh
```

or

```bash
sudo chown ubuntu deploy.sh
```

depending on the root cause.

---

# Interview Questions

### 1. Why do we need file permissions?

To protect files and directories from unauthorized access and control who can read, modify, or execute them.

---

### 2. What are the three basic Linux permissions?

* Read (`r`)
* Write (`w`)
* Execute (`x`)

---

### 3. What is the difference between permissions and ownership?

Permissions define what actions are allowed on a file.

Ownership defines which user owns the file and which group it belongs to.

---

### 4. Why is `777` considered dangerous?

Because it grants read, write, and execute permissions to everyone, creating significant security risks.

---

### 5. What is the difference between `chmod` and `chown`?

`chmod` changes permissions.

`chown` changes ownership.

---

### 6. What does `chgrp` do?

It changes the group ownership of a file or directory.

---

### 7. Why are groups useful in Linux?

Groups simplify administration by allowing permissions to be assigned to multiple users at once instead of configuring each user individually.

---

# Commands Practiced

```text
ls -l
chmod 755
chmod 644
chmod +x
chmod u+x
chmod g+w
chmod o-x
chmod -w
chown
chgrp
```

---

# Key Takeaways

* Linux uses file permissions to secure files and directories.
* Every file has an owner and a group.
* Permissions are divided into Owner, Group, and Others.
* Read = 4, Write = 2, Execute = 1.
* Common permission values include `755` and `644`.
* `chmod` changes permissions.
* `chown` changes the owner.
* `chgrp` changes the group.
* Understanding permissions and ownership is essential for troubleshooting Linux and DevOps environments.

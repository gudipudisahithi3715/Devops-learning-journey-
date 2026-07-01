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

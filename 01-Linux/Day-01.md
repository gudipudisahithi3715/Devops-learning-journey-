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

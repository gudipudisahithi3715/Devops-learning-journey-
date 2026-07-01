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

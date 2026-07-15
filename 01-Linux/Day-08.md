# 🐳 Day 8 - Docker Fundamentals 

# Docker Learning Journey

Before learning Docker, it is important to understand **why Docker was created**. Docker was not introduced because Virtual Machines were bad; it was introduced to solve the limitations of traditional deployment and virtualization.

---

# Traditional Application Deployment

## What is Traditional Deployment?

Before virtualization and containers existed, applications were installed **directly on a physical server**.

Example:

```
+--------------------------------+
| Physical Server                |
|--------------------------------|
| Operating System (Linux)       |
|--------------------------------|
| Java                           |
| MySQL                          |
| Apache                         |
| Payroll Application            |
+--------------------------------+
```

All applications shared the same operating system.

---

## Problems with Traditional Deployment

### 1. Dependency Conflicts

Suppose:

Application A requires Java 8.

Application B requires Java 21.

Only one version could usually be installed system-wide, causing compatibility issues.

---

### 2. Resource Wastage

Companies often purchased one physical server for each application.

Example:

```
Server 1 → HR Application

Server 2 → Banking Application

Server 3 → Payroll Application
```

Each server was expensive and often underutilized.

---

### 3. Poor Scalability

If an application suddenly received more traffic, the only solution was often to buy another physical server.

This process could take days or weeks.

---

### 4. Maintenance Cost

Every server required:

* Operating system updates
* Hardware maintenance
* Security patches
* Monitoring

This significantly increased operational costs.

---

# Why Virtualization?

Companies wanted to utilize hardware more efficiently.

Instead of:

```
1 Physical Server

↓

1 Application
```

They wanted:

```
1 Physical Server

↓

Multiple Applications
```

Virtualization made this possible.

---

# What is Virtualization?

Virtualization is the process of creating multiple virtual computers (Virtual Machines) on a single physical server.

Each Virtual Machine behaves like an independent computer.

---

## Architecture

```
+--------------------------------+
| Physical Server                |
+--------------------------------+
              │
              ▼
+--------------------------------+
| Hypervisor                     |
+--------------------------------+
      │        │         │
      ▼        ▼         ▼
+---------+ +---------+ +---------+
| VM 1    | | VM 2    | | VM 3    |
| Ubuntu  | | Windows | | RHEL    |
+---------+ +---------+ +---------+
```

Each VM has:

* CPU
* RAM
* Storage
* Operating System
* Applications

---

# What is a Virtual Machine?

A Virtual Machine (VM) is a software-based computer that behaves like a real physical computer.

Each VM has its own:

* Guest Operating System
* CPU allocation
* RAM allocation
* Storage
* Network configuration
* Installed software

Example:

```
VM 1

Ubuntu 24.04

Java 21

Spring Boot Application
```

Another VM:

```
VM 2

Windows Server

IIS

.NET Application
```

Both run independently on the same physical server.

---

# Characteristics of a Virtual Machine

✅ Independent Operating System

Each VM has its own guest OS.

---

✅ Independent Resources

Each VM has dedicated virtual resources.

Example:

VM 1

* 4 vCPUs
* 8 GB RAM

VM 2

* 2 vCPUs
* 4 GB RAM

---

✅ Isolation

If one VM crashes, the others continue running.

---

# Advantages of Virtualization

### Better Hardware Utilization

One physical server can host multiple virtual machines.

---

### Cost Savings

Fewer physical servers are required.

---

### Easy Backup and Recovery

Virtual Machines can be backed up and restored more easily than physical servers.

---

### Isolation

Applications running inside one VM do not directly affect other VMs.

---

# What is a Hypervisor?

A Hypervisor is software that creates and manages Virtual Machines.

It sits between the physical hardware and the virtual machines.

---

## Hypervisor Architecture

```
Physical Server

↓

Hypervisor

↓

Virtual Machines
```

The Hypervisor allocates:

* CPU
* RAM
* Storage
* Network

to each Virtual Machine.

---

# Types of Hypervisors

## Type 1 Hypervisor (Bare Metal)

Runs directly on physical hardware.

```
Hardware

↓

Hypervisor

↓

Virtual Machines
```

Examples:

* VMware ESXi
* Microsoft Hyper-V
* Xen

Advantages:

* Better performance
* Higher security
* Commonly used in production data centers

---

## Type 2 Hypervisor (Hosted)

Runs on top of an existing operating system.

```
Hardware

↓

Host Operating System

↓

Hypervisor

↓

Virtual Machines
```

Examples:

* Oracle VirtualBox
* VMware Workstation

Advantages:

* Easy to install
* Suitable for development and learning

---

# Problems with Virtual Machines

Although virtualization solved many problems, it introduced new challenges.

---

## 1. Guest Operating System Overhead

Every VM requires its own operating system.

Example:

```
VM 1

Ubuntu

------------

VM 2

Windows

------------

VM 3

RHEL
```

Each operating system consumes:

* RAM
* CPU
* Storage

---

## 2. Higher Resource Consumption

Running multiple operating systems requires significant hardware resources.

---

## 3. Slow Startup

Booting a Virtual Machine is similar to booting a real computer.

It may take several minutes.

---

## 4. Storage Usage

Each guest operating system occupies disk space.

Example:

Ubuntu OS: ~4 GB

Windows Server: ~15–20 GB

This increases storage requirements.

---

## 5. Maintenance Overhead

Each VM requires:

* OS updates
* Security patches
* Software maintenance

Managing many VMs becomes time-consuming.

---

# Real-World Example

Suppose a company has one physical server with:

* 32 CPU Cores
* 128 GB RAM

They create:

```
VM 1 → Ubuntu → Payroll

VM 2 → Windows → HR

VM 3 → RHEL → Analytics
```

Although all applications run on one physical server, each VM still consumes resources for its own operating system.

This limitation eventually led to the rise of **Containerization**, which we'll cover in Part 2.

---

# Key Takeaways

* Traditional deployment installs applications directly on physical servers.
* Virtualization allows multiple Virtual Machines to run on one physical server.
* Each VM has its own guest operating system.
* A Hypervisor creates and manages Virtual Machines.
* Type 1 Hypervisors run directly on hardware.
* Type 2 Hypervisors run on top of an operating system.
* The biggest drawback of Virtual Machines is the overhead of running a separate operating system for each VM.

---

# Interview Questions

### 1. What is Virtualization?

Virtualization is the process of creating multiple Virtual Machines on a single physical server using a Hypervisor.

---

### 2. What is a Virtual Machine?

A Virtual Machine is a software-based computer with its own operating system, CPU, memory, storage, and applications.

---

### 3. What is a Hypervisor?

A Hypervisor is software that creates and manages Virtual Machines by allocating hardware resources.

---

### 4. What are the two types of Hypervisors?

* Type 1 (Bare Metal)
* Type 2 (Hosted)

---

### 5. Why were containers introduced even though Virtual Machines already existed?

Because each Virtual Machine requires its own guest operating system, making VMs heavier, slower to start, and more resource-intensive.


# Containerization, Containers, Host OS, Namespaces & Cgroups

---

# Why Containerization?

In Part 1, we learned that Virtual Machines solved many problems, but they introduced new challenges:

* Every VM has its own Guest Operating System.
* Higher RAM and CPU consumption.
* Slower startup time.
* Increased storage requirements.
* More maintenance.

The IT industry needed a solution that provided the isolation of Virtual Machines **without requiring a separate operating system for every application**.

This led to **Containerization**.

---

# What is Containerization?

Containerization is a technology that packages an application along with everything it needs to run, while sharing the Host Operating System's kernel.

A container includes:

* Application Code
* Runtime (Java, Python, Node.js, etc.)
* Libraries
* Dependencies
* Configuration Files

A container **does not include a separate operating system**.

---

# Traditional VM vs Container

## Virtual Machine

```text
+-----------------------+
| Application           |
+-----------------------+
| Libraries             |
+-----------------------+
| Guest Operating System|
+-----------------------+
| Hypervisor            |
+-----------------------+
| Physical Server       |
+-----------------------+
```

Each VM has its own Guest OS.

---

## Container

```text
+-----------------------+
| Application           |
+-----------------------+
| Libraries             |
+-----------------------+
| Docker Engine         |
+-----------------------+
| Host Operating System |
+-----------------------+
| Physical Server       |
+-----------------------+
```

Containers share the Host OS kernel.

---

# What is a Container?

A Container is a lightweight, isolated runtime environment for an application.

It contains:

* Application
* Runtime
* Libraries
* Dependencies

It shares:

* Host OS Kernel

It does **not** contain:

* A separate operating system
* A separate kernel

---

# Example

Suppose your Host OS is Ubuntu.

```text
Ubuntu Host

↓

Docker Engine

↓

Container 1 → Java Application

Container 2 → Python Application

Container 3 → MySQL
```

All three containers share the Ubuntu kernel.

---

# Benefits of Containers

## Lightweight

Containers don't include a Guest OS.

Result:

* Less RAM usage
* Less Storage usage

---

## Fast Startup

Containers start in seconds.

Virtual Machines may take minutes to boot.

---

## Portability

A container runs the same way on:

* Developer Laptop
* Testing Server
* Production Server
* Cloud Platforms

---

## Consistency

"If it works on my laptop, it works in production."

This is one of Docker's biggest advantages.

---

# What is the Host OS?

The Host OS is **the operating system on which Docker Engine is installed.**

This is one of the most important Docker concepts.

---

## Scenario 1

Docker installed directly on a Physical Server.

```text
Physical Server

↓

Ubuntu

↓

Docker

↓

Containers
```

Here:

Host OS = Ubuntu

---

## Scenario 2

Docker installed inside a Virtual Machine.

```text
Physical Server

↓

VMware ESXi

↓

Ubuntu VM

↓

Docker

↓

Containers
```

Here:

Host OS = Ubuntu VM

Not VMware.

Not the Physical Server.

The Host OS is always where Docker Engine is installed.

---

# Which Kernel Do Containers Share?

Containers always share the **Host Operating System's kernel**.

Example:

```text
Ubuntu Host

↓

Docker

↓

Container A

Container B

Container C
```

All containers share the Ubuntu kernel.

---

# Multiple Virtual Machines

```text
Physical Server

↓

VMware

↓

VM1 → Ubuntu → Docker

↓

Container A

Container B

-------------------

VM2 → Ubuntu → Docker

↓

Container C

Container D
```

Container A and B share VM1's Ubuntu kernel.

Container C and D share VM2's Ubuntu kernel.

Containers across different VMs do **not** share kernels.

---

# Why Don't Containers Interfere?

Since containers share the same kernel, how do they remain isolated?

The answer is:

Linux provides:

* Namespaces
* Cgroups

Docker simply uses these Linux features.

---

# What are Namespaces?

Namespaces provide isolation.

Each container gets its own view of:

* Processes
* File System
* Network
* Hostname
* Users

Every container thinks it is running independently.

---

## Example

Container A

```text
/

app

logs
```

Container B

```text
/

python

models
```

Each container believes it owns its own filesystem.

---

# Process Namespace

Suppose:

Container A

```text
PID 1

↓

Java
```

Container B

```text
PID 1

↓

Python
```

Both have Process ID 1.

How?

Each container has its own Process Namespace.

They cannot see each other's processes.

---

# What are Cgroups?

Cgroups stands for **Control Groups**.

They manage and limit resource usage.

Docker uses Cgroups to control:

* CPU
* RAM
* Disk I/O
* Network usage

---

# Example

Container A

RAM Limit = 2 GB

If the application requests:

6 GB RAM

The Linux Kernel prevents it from exceeding 2 GB.

The application may receive an Out Of Memory (OOM) error or be terminated.

---

# Why Are Cgroups Important?

Imagine:

```text
Container 1 → Banking

Container 2 → Payment

Container 3 → Database
```

Without Cgroups:

The Payment container could consume all available memory.

As a result:

* Database crashes
* Banking application slows down

With Cgroups:

Each container receives its own resource limits.

---

# Docker Did Not Invent Containers

This is an important interview question.

Linux already had:

* Namespaces
* Cgroups

Docker made containerization easy by automating these technologies.

---

# VM vs Container

| Virtual Machine           | Container                 |
| ------------------------- | ------------------------- |
| Has Guest OS              | Shares Host OS Kernel     |
| Heavy                     | Lightweight               |
| Slower Startup            | Starts in Seconds         |
| Higher Storage Usage      | Lower Storage Usage       |
| Requires Hypervisor       | Requires Docker Engine    |
| More Resource Consumption | Less Resource Consumption |

---

# Real-Time Example

Suppose a company runs:

* Java Application
* Python Application
* MySQL Database

Using Virtual Machines:

```text
VM1 → Ubuntu → Java

VM2 → Ubuntu → Python

VM3 → Ubuntu → MySQL
```

Three Ubuntu operating systems.

Using Containers:

```text
Ubuntu Host

↓

Docker

↓

Java Container

Python Container

MySQL Container
```

Only one Ubuntu kernel is shared.

This saves memory, CPU, and storage.

---

# Key Takeaways

* Containers package applications with their dependencies.
* Containers share the Host Operating System kernel.
* The Host OS is where Docker Engine is installed.
* Namespaces provide isolation.
* Cgroups control resource usage.
* Docker uses Linux container technologies; it did not invent them.

---

# Interview Questions

### 1. What is Containerization?

Containerization is the process of packaging an application along with its dependencies into an isolated container while sharing the Host Operating System's kernel.

---

### 2. What is a Container?

A container is a lightweight, isolated runtime environment containing an application, its runtime, libraries, and dependencies.

---

### 3. What is Host OS?

The Host OS is the operating system on which Docker Engine is installed.

---

### 4. What do Containers share?

Containers share the Host Operating System's kernel.

---

### 5. What are Namespaces?

Namespaces isolate processes, networking, file systems, and other system resources between containers.

---

### 6. What are Cgroups?

Cgroups manage and limit resource usage such as CPU, memory, disk I/O, and network usage.

---

### 7. Can containers running on different Virtual Machines share the same kernel?

No. Containers only share the kernel of the Host Operating System where Docker Engine is installed.


# Docker, Docker Architecture, Images, Containers, Docker Hub & Registry

---

# What is Docker?

Docker is an **open-source containerization platform** that simplifies building, packaging, distributing, and running applications using containers.

Docker **did not invent containers**.

Linux already provided container technologies using:

* Namespaces
* Cgroups

Docker made them simple to use.

---

# Why was Docker Created?

Before Docker, creating Linux containers required manually configuring:

* Namespaces
* Cgroups
* Networking
* Storage
* Process Isolation

This required advanced Linux knowledge.

Docker automated all of these tasks.

Instead of configuring Linux manually, we simply execute:

```bash
docker run nginx
```

Docker performs all the complex work in the background.

---

# Responsibilities of Docker

Docker helps developers to:

* Build applications
* Package applications
* Ship applications
* Run applications

This ensures the same application works consistently across all environments.

---

# Docker vs Container

| Docker                          | Container            |
| ------------------------------- | -------------------- |
| Platform                        | Runtime Environment  |
| Builds and manages containers   | Runs the application |
| Uses Linux container technology | Uses Host OS Kernel  |
| Provides CLI and APIs           | Executes application |

Think of it like:

Docker = Factory

Container = Product

---

# Docker Architecture

Docker follows a Client-Server architecture.

```
                 User
                   │
                   │ docker run nginx
                   ▼
        +-----------------------+
        |    Docker Client      |
        +-----------------------+
                   │
           Docker REST API
                   │
                   ▼
        +-----------------------+
        |   Docker Daemon       |
        |      (dockerd)        |
        +-----------------------+
          │        │         │
          ▼        ▼         ▼
      Images   Containers Networks
          │
          ▼
      Docker Registry
     (Docker Hub / Private)
```

---

# Components of Docker Architecture

Docker Architecture consists of:

* Docker Client
* Docker Daemon
* Docker Engine
* Docker Images
* Docker Containers
* Docker Registry
* Docker Hub

---

# Docker Client

Docker Client is the interface through which users interact with Docker.

Whenever you execute commands like:

```bash
docker run

docker build

docker pull

docker images
```

you are interacting with the Docker Client.

The Docker Client **does not perform the actual work**.

It simply sends requests to the Docker Daemon.

---

## Real-Life Example

Customer

↓

Waiter

↓

Chef

The waiter takes your order but does not cook.

Similarly,

Docker Client accepts your command and forwards it.

---

# Docker Daemon (dockerd)

Docker Daemon is the background service responsible for performing Docker operations.

It can:

* Build Images
* Pull Images
* Push Images
* Create Containers
* Start Containers
* Stop Containers
* Remove Containers
* Create Networks
* Manage Volumes

Docker Daemon performs all the actual work.

---

## Restaurant Analogy

Customer

↓

Waiter

↓

Chef

Chef prepares the food.

Docker Daemon is the Chef.

---

# Docker Engine

Docker Engine is the complete Docker platform.

It consists of:

* Docker Client
* Docker Daemon
* Docker REST API

Together they form Docker Engine.

---

## Docker Engine Architecture

```
Docker Engine

↓

Docker Client

+

Docker Daemon

+

REST API
```

---

# Docker Image

A Docker Image is a **read-only blueprint** used to create containers.

It contains:

* Application
* Runtime
* Libraries
* Dependencies
* Configuration

Images are immutable.

Once created, they cannot be modified directly.

---

## Image Analogy

Recipe Book

↓

Cooked Food

Recipe = Image

Cooked Food = Container

---

# Docker Container

A Docker Container is a running instance of a Docker Image.

Example:

Image

```
nginx
```

Run:

```bash
docker run nginx
```

Docker creates:

Container

```
nginx Container
```

---

# One Image → Multiple Containers

```
           nginx Image
                │
      ┌─────────┼─────────┐
      ▼         ▼         ▼
Container1 Container2 Container3
```

One image can create multiple independent containers.

---

# Image vs Container

| Docker Image              | Docker Container      |
| ------------------------- | --------------------- |
| Blueprint                 | Running Instance      |
| Read Only                 | Read Write            |
| Static                    | Running               |
| Used to create containers | Executes applications |

---

# Docker Registry

Docker Registry is a storage system for Docker Images.

Its responsibility is:

* Store Images
* Distribute Images

Registries can be:

* Public
* Private

Examples:

* Docker Hub
* Amazon ECR
* Azure Container Registry
* Google Artifact Registry

---

# Docker Hub

Docker Hub is Docker's default public registry.

When Docker cannot find an image locally,

it automatically downloads it from Docker Hub.

Example:

```bash
docker run nginx
```

Docker checks:

```
Local Image Available?

↓

No

↓

Download from Docker Hub

↓

Create Container
```

---

# Docker Registry vs Docker Hub

| Docker Registry          | Docker Hub               |
| ------------------------ | ------------------------ |
| Generic Image Storage    | Docker's Public Registry |
| Can be Public or Private | Public by default        |
| Stores Images            | Stores Images            |
| Example: ECR, ACR        | Example: Docker Hub      |

---

# Complete Flow of docker run nginx

Suppose you execute:

```bash
docker run nginx
```

The complete workflow is:

Step 1

User executes command.

↓

Step 2

Docker Client receives the command.

↓

Step 3

Docker Client sends request to Docker Daemon using REST API.

↓

Step 4

Docker Daemon checks:

Is nginx image available locally?

↓

If YES

↓

Create Container

↓

Start Container

↓

Application Running

If NO

↓

Connect to Docker Hub

↓

Download Image

↓

Store Image Locally

↓

Create Container

↓

Start Container

---

## Complete Workflow Diagram

```
User

↓

Docker Client

↓

REST API

↓

Docker Daemon

↓

Local Images

↓

Image Found?

│

├── Yes

│

▼

Create Container

↓

Run Application

│

└── No

↓

Docker Hub

↓

Download Image

↓

Create Container

↓

Application Running
```

---

# Real-Time Example

Developer creates:

```
payment-app:v1
```

Pushes image:

```
Docker Hub
```

Production Server executes:

```bash
docker pull payment-app:v1

docker run payment-app:v1
```

The same image runs successfully in production.

---

# Key Takeaways

* Docker is a platform for containerization.
* Docker did not invent containers.
* Docker Client accepts user commands.
* Docker Daemon performs all Docker operations.
* Docker Engine = Client + Daemon + REST API.
* Docker Images are blueprints.
* Docker Containers are running instances.
* Docker Registry stores Docker Images.
* Docker Hub is Docker's default public registry.

---

# Interview Questions

### 1. What is Docker?

Docker is an open-source containerization platform used to build, package, distribute, and run applications in containers.

---

### 2. Did Docker invent containers?

No. Docker simplified Linux container technology built using Namespaces and Cgroups.

---

### 3. What is Docker Client?

Docker Client is the command-line interface that accepts Docker commands and forwards them to Docker Daemon.

---

### 4. What is Docker Daemon?

Docker Daemon is the background service responsible for creating containers, building images, managing networks, and performing Docker operations.

---

### 5. What is Docker Engine?

Docker Engine consists of Docker Client, Docker Daemon, and Docker REST API.

---

### 6. What is a Docker Image?

A Docker Image is a read-only blueprint containing an application and its dependencies.

---

### 7. What is a Docker Container?

A Docker Container is a running instance of a Docker Image.

---

### 8. Can one Docker Image create multiple Containers?

Yes.

---

### 9. What is Docker Registry?

Docker Registry is a service used to store and distribute Docker Images.

---

### 10. What is Docker Hub?

Docker Hub is Docker's default public registry from which Docker downloads images if they are not available locally.


# Docker Workflow, Docker Editions, Production Scenarios, Interview Questions & Revision

---

# Complete Docker Workflow

Let's understand how Docker is used in a real software development lifecycle.

## Step 1 – Developer Writes Code

Example:

```text
Spring Boot Application
```

The developer writes the application and tests it locally.

---

## Step 2 – Create Dockerfile

A Dockerfile describes how to package the application.

Example:

```dockerfile
FROM openjdk:21

COPY target/app.jar app.jar

CMD ["java","-jar","app.jar"]
```

*(We'll study Dockerfiles in detail later.)*

---

## Step 3 – Build Docker Image

```bash
docker build -t payment-service:v1 .
```

Docker reads the Dockerfile and creates an image.

Result:

```text
payment-service:v1
```

---

## Step 4 – Push Image to Registry

```bash
docker push payment-service:v1
```

The image is uploaded to:

* Docker Hub
* Amazon ECR
* Azure Container Registry
* Private Registry

---

## Step 5 – Production Server Pulls Image

```bash
docker pull payment-service:v1
```

The server downloads the exact image built by the developer.

---

## Step 6 – Run Container

```bash
docker run payment-service:v1
```

Docker creates a container from the image and starts the application.

---

# Complete Lifecycle

```text
Developer

↓

Write Code

↓

Dockerfile

↓

docker build

↓

Docker Image

↓

docker push

↓

Docker Registry

↓

Production Server

↓

docker pull

↓

Docker Image

↓

docker run

↓

Container

↓

Application Running
```

---

# Why This Workflow Matters

Without Docker:

Developer sends:

* Source Code
* Installation Guide
* Java Version
* Library List
* Configuration Instructions

Operations Team performs manual setup.

---

With Docker:

Developer shares only:

```text
payment-service:v1
```

The application runs the same way in every environment.

---

# Docker Engine vs Docker Desktop

## Docker Engine

Docker Engine is the core runtime responsible for:

* Building Images
* Running Containers
* Managing Networks
* Managing Volumes

Typically installed on Linux servers.

---

## Docker Desktop

Docker Desktop is a desktop application for:

* Windows
* macOS

It includes:

* Docker Engine
* Docker CLI
* Docker Compose
* GUI
* WSL2 Integration (Windows)

Used mainly by developers.

---

# Docker CE (Community Edition)

Docker Community Edition is the free version of Docker.

Suitable for:

* Students
* Developers
* Most production workloads

It includes all the core Docker features required for learning and development.

---

# Real Production Scenario

Suppose an e-commerce company has:

* Payment Service
* Order Service
* Inventory Service
* Notification Service

Each service is packaged as its own Docker image.

```text
payment:v1

order:v1

inventory:v1

notification:v1
```

Production servers pull the required images and run containers.

Benefits:

* Consistent deployments
* Easy rollbacks
* Independent scaling
* Faster releases

---

# Common Advantages of Docker

✅ Lightweight

✅ Fast startup

✅ Portability

✅ Environment consistency

✅ Better resource utilization

✅ Easy deployment

✅ Simplified CI/CD integration

---

# Docker in DevOps Pipeline

```text
Developer

↓

Git Commit

↓

GitHub

↓

Jenkins Pipeline

↓

Docker Build

↓

Docker Image

↓

Docker Registry

↓

Kubernetes

↓

Production
```

This is why learning Git before Docker hands-on is important.

---

# Quick Revision

## Traditional Deployment

* Applications installed directly on physical servers.
* Dependency conflicts.
* Poor hardware utilization.

---

## Virtualization

* Multiple Virtual Machines on one physical server.
* Each VM has its own Guest OS.

---

## Containers

* Lightweight
* Share Host OS Kernel
* Faster than VMs

---

## Namespaces

Provide isolation for:

* Processes
* Network
* File System
* Hostname

---

## Cgroups

Manage:

* CPU
* RAM
* Disk I/O
* Network

---

## Docker

Docker simplifies container creation and management.

---

## Docker Architecture

Docker Client

↓

REST API

↓

Docker Daemon

↓

Images

↓

Containers

↓

Docker Registry

---

## Docker Image

Blueprint for creating containers.

---

## Docker Container

Running instance of an image.

---

## Docker Registry

Stores Docker images.

---

## Docker Hub

Default public Docker Registry.

---

# Frequently Asked Interview Questions

### 1. What is Docker?

Docker is an open-source platform used to build, package, distribute, and run applications in containers.

---

### 2. Why was Docker introduced?

To simplify containerization and eliminate the complexity of manually configuring Linux container technologies.

---

### 3. Did Docker invent containers?

No. Linux already provided Namespaces and Cgroups. Docker simplified their usage.

---

### 4. What is the difference between Virtual Machines and Containers?

Virtual Machines have their own Guest Operating System.

Containers share the Host Operating System kernel.

---

### 5. What is the Host OS?

The operating system on which Docker Engine is installed.

---

### 6. What is Docker Engine?

Docker Engine consists of:

* Docker Client
* Docker Daemon
* REST API

---

### 7. What is Docker Daemon?

The background service that performs Docker operations.

---

### 8. What is Docker Client?

The interface through which users execute Docker commands.

---

### 9. What is Docker Image?

A read-only blueprint used to create containers.

---

### 10. What is Docker Container?

A running instance of a Docker Image.

---

### 11. Can one image create multiple containers?

Yes.

---

### 12. What is Docker Registry?

A service that stores Docker images.

---

### 13. What is Docker Hub?

Docker's default public image registry.

---

### 14. What happens when we execute `docker run nginx`?

1. Docker Client receives the command.
2. Sends it to Docker Daemon.
3. Daemon checks for the image locally.
4. If unavailable, downloads it from Docker Hub.
5. Creates a container.
6. Starts the container.

---

### 15. Why are containers lightweight?

Because they share the Host Operating System kernel instead of running a separate Guest Operating System.

---

# Important Diagrams

## VM Architecture

```text
Application

↓

Guest OS

↓

Hypervisor

↓

Hardware
```

---

## Container Architecture

```text
Application

↓

Libraries

↓

Docker Engine

↓

Host OS Kernel

↓

Hardware
```

---

## Docker Architecture

```text
User

↓

Docker Client

↓

REST API

↓

Docker Daemon

↓

Images

↓

Containers

↓

Docker Registry
```

---

# Cheat Sheet

| Component        | Purpose                    |
| ---------------- | -------------------------- |
| Docker Client    | Accepts user commands      |
| Docker Daemon    | Performs Docker operations |
| Docker Engine    | Complete Docker platform   |
| Docker Image     | Blueprint                  |
| Docker Container | Running instance           |
| Docker Registry  | Stores images              |
| Docker Hub       | Public registry            |
| Namespaces       | Isolation                  |
| Cgroups          | Resource limits            |
| Host OS          | OS running Docker Engine   |

---

# Key Takeaways

---

* Containers are lightweight because they share the Host OS kernel.
* Docker automates Linux container technologies.
* Docker Architecture is one of the most important interview topics.
* Images are blueprints; containers are running instances.
* Registries store images; Docker Hub is the default public registry.

---


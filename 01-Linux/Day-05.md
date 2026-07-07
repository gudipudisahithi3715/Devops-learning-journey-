# Day 05 - Linux Networking Fundamentals

#  1. What is Networking?
### Theory

Networking is the process of connecting two or more systems to communicate and exchange data.

Without networking, computers cannot share information, access websites, or communicate with servers.

###  Real-Time DevOps Example

A developer pushes code to GitHub.

The Jenkins server pulls that code over the network.

Without networking, CI/CD pipelines cannot work.

#  2. What is an IP Address?
###  Theory

An IP (Internet Protocol) Address is a unique address assigned to every device connected to a network.

It allows computers to identify and communicate with each other.

Think of an IP address like a house address. Just as a courier uses your home address to deliver a package, computers use IP addresses to send and receive data.

Example:

192.168.1.10
#   3. Types of IP Addresses
###  Public IP Address

A Public IP is accessible over the Internet.

Example:

AWS EC2 Public IP
Company Website
###  Private IP Address

A Private IP is used inside a local network.

Private IP ranges:

10.0.0.0 – 10.255.255.255

172.16.0.0 – 172.31.255.255

192.168.0.0 – 192.168.255.255
###  Real-Time DevOps Example

An EC2 instance may have:

Public IP → Accessible from the Internet.
Private IP → Used for communication with databases or other servers inside the VPC.
#   4. Static vs Dynamic IP
###  Static IP
Does not change.
Used for production servers.
Suitable for web servers, Jenkins, databases, and Kubernetes master nodes.
###  Dynamic IP
Assigned automatically.
Can change after reconnecting or rebooting.
Commonly used for laptops, phones, and Wi-Fi clients.
###  Real-Time DevOps Example

Production servers use Static IP because applications and DNS records depend on a fixed address.

#   5. Hostname
###  Theory

A Hostname is the human-readable name assigned to a computer.

Instead of identifying a system by its IP address, we can identify it using its hostname.

Example:

jenkins-server

ubuntu-dev

web-server
###  Commands

Display Hostname

hostname

Change Hostname

sudo hostnamectl set-hostname server-name
#   6. DNS (Domain Name System)
###  Theory

DNS (Domain Name System) is a service that converts a human-readable domain name into an IP address.

Humans remember names.

Computers communicate using IP addresses.

DNS acts as a translator between them.

Example:

google.com

↓

142.xxx.xxx.xxx
###  Why Do We Need DNS?

Without DNS, users would have to remember IP addresses for every website.

Instead of typing:

142.xxx.xxx.xxx

we simply type:

google.com

DNS performs the translation automatically.

#   7. DNS Resolution Process

When you enter:

www.google.com

The process is:

User

↓

Browser

↓

Checks Cache

↓

DNS Resolver

↓

DNS Server

↓

Returns IP Address

↓

Browser connects to Server

↓

Website Opens
#  8. DNS Records
###  A Record

Maps a domain name to an IPv4 address.

Example

app.company.com

↓

52.66.xxx.xxx
###  CNAME Record

Maps one domain name to another domain name.

Example

www.company.com

↓

company.com
###  MX Record

Specifies the mail server responsible for receiving emails.

Without MX records, emails cannot be delivered correctly.

###  TXT Record

Used for:

Domain Verification
SPF
DKIM
Security Verification
###  NS Record

Specifies the authoritative DNS servers for a domain.

Example

ns1.provider.com

ns2.provider.com
#   DNS Commands

### Check DNS Resolution

nslookup google.com

### Detailed DNS Information

dig google.com

### Another DNS Lookup Tool

host google.com
###  Real-Time DevOps Example

Suppose your application is not opening.

First check DNS:

nslookup app.company.com

If the domain resolves correctly, move on to checking network connectivity and the application itself.

#   Interview Questions
### Q1. What is Networking?

Answer:

Networking is the communication between two or more systems to exchange data and resources.

### Q2. What is an IP Address?

Answer:

An IP Address is a unique address assigned to every device on a network that allows devices to communicate.

### Q3. Difference between Public and Private IP?

Answer:

Public IP is accessible over the Internet.
Private IP is used within a private network.
### Q4. Why do production servers use Static IP?

Answer:

Production servers require a fixed IP address so applications, DNS records, and clients can reliably connect to them.

### Q5. What is a Hostname?

Answer:

A Hostname is a human-readable name assigned to a computer for easy identification.

### Q6. What is DNS?

Answer:

DNS (Domain Name System) translates human-readable domain names into IP addresses.

### Q7. Why do we need DNS?

Answer:

Humans remember domain names easily, while computers communicate using IP addresses. DNS translates between them.

### Q8. Explain DNS Resolution.

Answer:

The browser checks its cache. If the IP isn't found, the request goes to the DNS Resolver, then to the DNS Server, which returns the IP address. The browser then connects to the server using that IP.

### Q9. What is an A Record?

Answer:

An A Record maps a domain name to an IPv4 address.

### Q10. What is a CNAME Record?

Answer:

A CNAME Record maps one domain name to another domain name.

### Q11. What is an MX Record?

Answer:

An MX Record specifies the mail server responsible for receiving emails.

### Q12. What is a TXT Record?

Answer:

A TXT Record stores text information for domain verification, SPF, DKIM, and other security-related purposes.

### Q13. What is an NS Record?

Answer:

An NS Record specifies the authoritative DNS servers for a domain.

### Q14. Which command checks DNS resolution?

Answer:

nslookup google.com
# Network Interface (NIC)
### Theory

A Network Interface is the connection point through which a computer communicates with a network.

It can be:

Physical (Ethernet Card, Wi-Fi Card)
Virtual (Docker Bridge, Kubernetes Network Interface, VPN Interface)

Without a network interface, a computer cannot send or receive data.

Think of it as the door of a house. Without a door, people cannot enter or leave. Similarly, without a network interface, data cannot enter or leave the system.

#  Network Interface Card (NIC)
### Theory

A NIC (Network Interface Card) is the hardware (or virtual hardware) that connects a computer to a network.

Every NIC has:

IP Address
MAC Address
Interface Name

### Examples:

Ethernet Card
Wi-Fi Adapter
AWS EC2 Virtual NIC
Docker Virtual Interface
#  Common Interface Names

### Examples:

eth0
eth1
ens33
ens5
enp0s3
wlan0
lo
docker0
Interface	Purpose
lo	Loopback Interface
eth0	Ethernet Interface (Old Naming)
ens5	Ethernet Interface (Modern Naming)
wlan0	Wireless Interface
docker0	Docker Bridge Interface
#  10. Loopback Interface
###  Theory

The Loopback Interface is a virtual network interface that allows a computer to communicate with itself.

Loopback IP Address:

127.0.0.1

or

localhost

Both refer to the same machine.

###  Real-Time DevOps Example

Suppose Jenkins is running on your laptop.

Instead of opening:

http://192.168.1.10:8080

You can simply use:

http://localhost:8080

because the request never leaves your own computer.

#  View Network Interfaces

Display IP Address of Interfaces

ip addr

or

ip a

Shows:

Interface Name
IP Address
MAC Address
Interface Status
###  Check Interface Status
ip link

Shows:

UP / DOWN Status
Interface Name
MAC Address

Example

ens5 UP
lo UP
#  Routing Table
###  Theory

Whenever a system sends data, Linux checks the Routing Table to decide where the packet should go.

The routing table contains the paths used to reach different networks.

View Routing Table
ip route

Example

default via 172.31.0.1 dev ens5

172.31.0.0/20 dev ens5
#  Default Gateway
###  Theory

The Default Gateway is the router through which the system sends traffic destined for other networks or the Internet.

Without a default gateway:

The system can communicate within its local network.
It cannot communicate with systems on other networks or access the Internet.
###  Real-Time DevOps Example

An EC2 instance cannot access GitHub.

Check:

ip addr

Is an IP address assigned?

Then check:

ip route

Is there a default gateway?

If the default route is missing, the server cannot reach the Internet.

#  Difference Between Networking Commands
Command	Purpose
ip addr	Shows IP Address of Interfaces
ip link	Shows Interface Status
ip route	Shows Routing Table
#  11. Ping Command
###  Theory

The ping command is used to check network connectivity between two systems.

It sends an ICMP Echo Request packet.

If the destination is reachable, it replies with an ICMP Echo Reply.

Using ping we can check:

Network Connectivity
Response Time (Latency)
Packet Loss
Basic Reachability
#  ICMP
###  Theory

ICMP stands for:

Internet Control Message Protocol

It is used for:

Network Diagnostics
Error Reporting
Connectivity Testing

Unlike TCP and UDP, ICMP is not used for transferring application data.

#  Ping Commands

Ping Domain

ping google.com

Ping IP

ping 8.8.8.8

Send Only 4 Packets

ping -c 4 google.com
#  Understanding Ping Output

### Example

64 bytes from 8.8.8.8:
icmp_seq=1
ttl=117
time=14 ms
64 bytes

Amount of data received.

### icmp_seq

Sequence number of packets.

### ttl

Time To Live.

It limits how many routers a packet can pass through before being discarded.

### time

Round Trip Time (Latency).

Lower latency means faster communication.

#  Packet Loss
###  Theory

Packet Loss occurs when packets sent by the source do not reach the destination.

### Example

Packets Sent : 4

Packets Received : 4

Packet Loss : 0%

Good Network

### Example

Packets Sent : 4

Packets Received : 2

Packet Loss : 50%

Poor Network

Causes of Packet Loss
Network Congestion
Faulty Cable
Wi-Fi Interference
Firewall Blocking ICMP
Server Down
Router Failure
###  Real-Time DevOps Example

Application not opening.

Check:

ping app.company.com

If this fails, check:

DNS
Routing
Firewall
Server Status

If:

ping google.com

fails but

ping 8.8.8.8

works,

then the problem is DNS, not network connectivity.

#  12. curl Command
###  Theory

curl (Client URL) is used to transfer data between a client and a server.

DevOps Engineers use it to:

Test APIs
Verify Websites
Check Application Health
Test Load Balancers
Verify Kubernetes Services

Unlike ping, curl checks whether the application is responding.

#  Common curl Commands

### Get Website Response

curl https://google.com

### Show Only Headers

curl -I https://google.com

### Follow Redirects

curl -L http://google.com

### Save Output

curl -o output.html https://example.com
#  Common HTTP Status Codes
Status	Meaning
200	OK
201	Created
301	Permanent Redirect
302	Temporary Redirect
400	Bad Request
401	Unauthorized
403	Forbidden
404	Not Found
500	Internal Server Error
503	Service Unavailable
###  Real-Time DevOps Example

A website is not opening.

Instead of checking only:

ping app.company.com

Use:

curl -I https://app.company.com

If you receive:

HTTP/1.1 200 OK

The application is working.

#  13. wget Command
###  Theory

wget is a command-line utility used to download files from the Internet.

Supported Protocols:

HTTP
HTTPS
FTP
#  wget Commands

### Download File

wget https://example.com/file.zip

### Save with Custom Name

wget -O jenkins.war https://updates.jenkins.io/download/jenkins.war

### Resume Download

wget -c https://example.com/file.iso
💼 Real-Time DevOps Example

### Download Jenkins WAR file directly to the Linux server:

wget https://get.jenkins.io/war/latest/jenkins.war
#  Difference Between curl and wget
curl	wget
Tests APIs	Downloads Files
Returns Response	Saves Files
Used for HTTP Requests	Used for File Downloads
Used for Health Checks	Used for Software Installation
# Interview Questions & Answers
### Q1. What is a Network Interface?

Answer:

A Network Interface is the connection point through which a computer communicates with a network. It can be physical (Ethernet/Wi-Fi) or virtual (Docker bridge, VPN, Loopback).

### Q2. What is a NIC?

Answer:

NIC (Network Interface Card) is the hardware or virtual hardware that connects a computer to a network. Every NIC has an IP address, MAC address, and interface name.

### Q3. What information does a NIC contain?

Answer:

A NIC contains:

IP Address
MAC Address
Interface Name
### Q4. What is the Loopback Interface?

Answer:

The Loopback Interface is a virtual network interface used by a computer to communicate with itself. It is mainly used for testing and accessing local services.

### Q5. What is the IP address of the Loopback Interface?

Answer:

The Loopback IP address is:

127.0.0.1

It can also be accessed using:

localhost
### Q6. Which command displays IP addresses assigned to network interfaces?

Answer:

ip addr

or

ip a
### Q7. Which command displays the status of network interfaces?

Answer:

ip link

It shows whether an interface is UP or DOWN along with its MAC address.

### Q8. Which command displays the routing table?

Answer:

ip route

It displays the routing table and the default gateway used by the system.

### Q9. What is a Default Gateway?

Answer:

A Default Gateway is the router through which a computer sends traffic to other networks or the Internet. It acts as the exit point from the local network.

### Q10. What happens if the Default Gateway is missing?

Answer:

If the Default Gateway is missing:

The system can communicate with devices in the same local network.
It cannot communicate with devices on other networks or access the Internet.
### Q11. What is the ping command used for?

Answer:

The ping command is used to check network connectivity between two systems. It verifies whether a remote host is reachable and provides information such as latency and packet loss.

### Q12. Which protocol does ping use?

Answer:

ping uses ICMP (Internet Control Message Protocol).

It sends an ICMP Echo Request and waits for an ICMP Echo Reply.

### Q13. What is Packet Loss?

Answer:

Packet Loss occurs when one or more packets sent from the source do not reach the destination or no reply is received.

Common causes include:

Network congestion
Faulty cables
Firewall blocking ICMP
Server or router issues
### Q14. What is Latency?

Answer:

Latency is the time taken for a packet to travel from the source to the destination and back again (Round Trip Time). It is usually measured in milliseconds (ms).

Lower latency indicates a faster network connection.

### Q15. What is curl?

Answer:

curl (Client URL) is a command-line tool used to transfer data between a client and a server. It is commonly used to test websites, APIs, and application health.

### Q16. Why is curl preferred over ping for testing web applications?

Answer:

ping only checks network connectivity using ICMP.

curl sends HTTP/HTTPS requests and verifies whether the web server or application is actually responding.

Therefore, curl is preferred for testing websites and APIs.

### Q17. What is wget?

Answer:

wget is a command-line utility used to download files from the Internet using HTTP, HTTPS, or FTP protocols.

It is commonly used to download software packages, configuration files, and installation files.

### Q18. What is the difference between curl and wget?

Answer:

curl	wget
Used to transfer data between client and server	Used mainly to download files
Commonly used to test APIs and websites	Commonly used to download files and software
Returns the server response	Saves the downloaded file to disk
Useful for API testing	Useful for downloading packages and installers
#  Scenario-Based Interview Question
### Q19. Your server cannot access GitHub. How would you troubleshoot?

Answer:

I would troubleshoot in the following order:

Check whether the network interface is up:

ip link

Check if an IP address is assigned:

ip addr

Verify the routing table and default gateway:

ip route

Test connectivity:

ping 8.8.8.8

Test DNS resolution:

nslookup github.com

Verify application access:

curl -I https://github.com
# 14. ss Command (Socket Statistics)
### Theory

The ss (Socket Statistics) command is used to display:

Active Network Connections
Listening Ports
TCP Connections
UDP Connections
Socket Information

It is the modern replacement for the netstat command.

#  Why Do DevOps Engineers Use ss?

DevOps Engineers use ss to:

Check if an application is running.
Verify whether an application is listening on the expected port.
Identify which process is using a specific port.
Troubleshoot network-related issues.
###  Common Commands

### Display all listening TCP and UDP ports

ss -tuln
Options
t → TCP
u → UDP
l → Listening
n → Numeric Port Numbers

### Display the process using a port

sudo ss -tulnp

Example Output

LISTEN 0 128 *:8080

users:(("java",pid=2456))

This shows:

Port Number
Process Name
PID

### Display established TCP connections

ss -ta
###  Real-Time DevOps Example

Users report Jenkins is not opening.

Check:

sudo ss -tulnp

If port 8080 is missing,

Jenkins is probably not running.

#  15. Port Numbers
###  Theory

A Port is a logical communication endpoint used by applications on a computer.

Think of it like an apartment building.

IP Address → Building Address
Port Number → Apartment Number

The IP identifies the computer.

The Port identifies the application running on that computer.

#  Common DevOps Ports
Port	Service
22	SSH
21	FTP
25	SMTP
53	DNS
80	HTTP
443	HTTPS
3306	MySQL
5432	PostgreSQL
6379	Redis
8080	Jenkins
9000	SonarQube
9090	Prometheus
3000	Grafana
6443	Kubernetes API Server
###  Real-Time DevOps Example

If Jenkins is running but users cannot access it:

Check whether port 8080 is listening.

sudo ss -tulnp
#  16. netstat
###  Theory

netstat is an older Linux command used to display:

Network Connections
Listening Ports
Routing Table
Network Statistics

Today, ss is preferred because it is faster and provides more detailed information.

###  Commands

Display listening ports

netstat -tuln

Display all active connections

netstat -an
#  Difference Between ss and netstat
ss	netstat
Modern command	Older command
Faster	Slower
More detailed	Less detailed
Preferred today	Used in legacy systems
#  17. SSH (Secure Shell)
###  Theory

SSH (Secure Shell) is a secure network protocol used to remotely access and manage Linux servers.

Instead of physically logging into a server, DevOps Engineers connect remotely using SSH.

SSH encrypts all communication between the client and the server.

#  Why Do DevOps Engineers Use SSH?
Access AWS EC2 Instances
Configure Linux Servers
Deploy Applications
Restart Services
Troubleshoot Production Issues
#  Default SSH Port
22
###  Connect to a Server
ssh username@server-ip

Example

ssh ubuntu@54.xx.xx.xx
Password Authentication

Login using:

Username
Password
Key-Based Authentication

Instead of passwords, SSH uses:

Private Key (Client)
Public Key (Server)

This is more secure and commonly used in production.

### Generate SSH Keys
ssh-keygen

Creates

~/.ssh/id_rsa

Private Key

~/.ssh/id_rsa.pub

Public Key

Copy Public Key
ssh-copy-id ubuntu@server-ip

Now passwordless login is enabled.

###  Real-Time DevOps Example

Every day DevOps Engineers SSH into EC2 instances to:

Deploy Applications
Restart Services
Check Logs
Configure Servers
#  18. SCP (Secure Copy)
###  Theory

scp is used to securely copy files between Linux systems using SSH.

### Copy Local File to Remote Server
scp file.txt ubuntu@server-ip:/home/ubuntu
### Copy Remote File to Local Machine
scp ubuntu@server-ip:/home/ubuntu/file.txt .
###  Real-Time DevOps Example

Deploy an Nginx configuration file:

scp nginx.conf ubuntu@54.xx.xx.xx:/etc/nginx/
#  19. Firewall
###  Theory

A Firewall controls incoming and outgoing network traffic based on predefined security rules.

It protects servers from unauthorized access.

###  Why is Firewall Important?

Without a firewall:

Unauthorized users may access services.
Servers become vulnerable.

With a firewall:

Only required ports are allowed.
Unnecessary ports remain blocked.
###  UFW (Ubuntu Firewall)

### Check Status

sudo ufw status

### Enable Firewall

sudo ufw enable

### Allow SSH

sudo ufw allow 22

### Allow HTTP

sudo ufw allow 80

### Allow HTTPS

sudo ufw allow 443

### Block Port

sudo ufw deny 8080
###  Real-Time DevOps Example

Website is not opening.

Check:

sudo ufw status

If port 80 is blocked:

sudo ufw allow 80

Website becomes accessible.

#  Interview Questions & Answers
### Q1. What is the ss command?

Answer:

ss (Socket Statistics) is used to display active network connections, listening ports, TCP/UDP sockets, and socket statistics.

### Q2. Why is ss preferred over netstat?

Answer:

ss is faster, more efficient, and provides more detailed socket information than netstat. It is the modern replacement for netstat.

### Q3. What does ss -tuln display?

Answer:

It displays all listening TCP and UDP ports with numeric port numbers.

### Q4. Which command shows the process using a port?

Answer:

sudo ss -tulnp
### Q5. What is a Port?

Answer:

A Port is a logical communication endpoint that identifies a specific application or service running on a computer.

### Q6. What is the difference between an IP Address and a Port?

Answer:

An IP Address identifies the computer on the network.

A Port identifies the application or service running on that computer.

### Q7. Name five commonly used DevOps ports.

Answer:

22 → SSH
80 → HTTP
443 → HTTPS
8080 → Jenkins
6443 → Kubernetes API Server
### Q8. Which port does Jenkins use by default?

Answer:

Port 8080.

### Q9. Which port does SSH use?

Answer:

Port 22.

### Q10. Which ports are used by HTTP and HTTPS?

Answer:

HTTP → Port 80
HTTPS → Port 443
### Q11. What is SSH?

Answer:

SSH (Secure Shell) is a secure protocol used to remotely access and manage Linux servers over a network.

### Q12. Why is SSH preferred over Telnet?

Answer:

SSH encrypts communication, making it secure. Telnet sends data in plain text and is not secure.

### Q13. What is the difference between Password Authentication and Key-Based Authentication?

Answer:

Password Authentication uses a username and password.

Key-Based Authentication uses a public-private key pair and is more secure.

### Q14. What is scp?

Answer:

scp (Secure Copy) is used to securely transfer files between systems over SSH.

### Q15. What is a Firewall?

Answer:

A Firewall is a security mechanism that controls incoming and outgoing network traffic based on predefined rules.

### Q16. Why do companies use Firewalls?

Answer:

Companies use firewalls to prevent unauthorized access, improve security, and allow only trusted traffic to reach servers.

### Q17. Which command checks the firewall status on Ubuntu?

Answer:

sudo ufw status
### Q18. Which command allows HTTP traffic?

Answer:

sudo ufw allow 80
### Q19. A server is running, but users cannot access it. What would you check?

Answer:

1. Check if the application/service is running.

2. Verify that the application is listening on the expected port using:

sudo ss -tulnp

3. Check firewall rules using:

sudo ufw status

4. Test the application using:

curl http://server-ip:port

5. Review the application logs.

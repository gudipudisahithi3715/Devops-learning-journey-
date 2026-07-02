# Day 2 - Linux Users & Groups

---

# 1. What are Linux Users?

Linux is a multi-user operating system, meaning multiple users can access and work on the same system simultaneously. Every user has a unique User ID (UID), a home directory, a login shell, and specific permissions that determine what they can access.

Linux uses users to provide security, accountability, and controlled access to system resources.

---

# 2. What are the Different Types of Users?

Linux has three types of users.

### Root User
- Administrator of the Linux system.
- Has complete control over all files and settings.
- Can perform any administrative task.

### Normal User
- Used for day-to-day work.
- Has limited permissions.
- Requires `sudo` for administrative tasks.

### System User
- Created for applications and services like Jenkins, MySQL, and Nginx.
- Usually uses `/usr/sbin/nologin`.
- Improves security by preventing interactive login.

---

# 3. Why Do We Need Different Types of Users?

Giving root access to every user is dangerous because anyone could modify critical system files.

Linux follows the **Principle of Least Privilege (PoLP)**, which means users receive only the permissions required to perform their tasks.

---

# 4. What are Linux Groups?

A group is a collection of users.

Instead of assigning permissions individually to every user, administrators assign permissions to a group and add users to that group.

This makes administration easier and more secure.

---

# 5. Types of Groups

### Primary Group
Every user has one primary group. Files created by the user belong to this group by default.

### Secondary (Supplementary) Groups
A user can belong to multiple secondary groups to gain additional permissions.

---

# 6. Why are Groups Important?

Suppose a company hires 50 DevOps Engineers.

Instead of assigning Docker permissions to each engineer individually, the administrator creates a **docker** group, grants permissions to the group, and adds all engineers to it.

This simplifies administration and follows the Principle of Least Privilege.

---

# 7. Common User Commands

Create a user

```bash
sudo useradd username

Change password

passwd
sudo passwd username

Display current user

whoami

Display user details

id

Display groups

groups

Display logged-in users

who
8. What is usermod?

usermod is used to modify an existing user account.

Examples:

Change login shell

sudo usermod -s /bin/bash username

Change home directory

sudo usermod -d /new/home -m username

Add user to a group

sudo usermod -aG docker username

Important: Always use -aG because -G replaces all supplementary groups.

9. What is userdel?

Delete only the user account

sudo userdel username

Delete the user account along with the home directory and mail spool

sudo userdel -r username
10. Group Management

Create a group

sudo groupadd groupname

Delete a group

sudo groupdel groupname

A group cannot be deleted if it is still the primary group of a user.

11. Important Linux Files
/etc/passwd

Stores:

Username
UID
GID
Home Directory
Login Shell

It does not store passwords.

/etc/shadow

Stores:

Password hash
Password aging
Password expiry
Warning period

Only the root user can read this file.

/etc/group

Stores:

Group Name
Group ID (GID)
Group Members
12. Hashing vs Encryption
Hashing
One-way process.
Cannot be reversed.
Used for storing passwords.
Encryption
Two-way process.
Can be decrypted using a key.
Used for protecting recoverable data.
13. Real-Time DevOps Examples
Docker Permission Denied

Instead of giving root access:

sudo usermod -aG docker username

Add the user to the Docker group.

Employee Leaves the Company

Best practice:

Lock the account.
Retain files.
Back up important data.
Delete the account after the retention period.
14. Interview Questions
Q1. What are the different types of users in Linux?

Answer:

Linux has three main types of users:

Root User: Administrator with complete system access.
Normal User: Regular user with limited permissions.
System User: Created for services like Jenkins, MySQL, and Nginx. These users usually cannot log in interactively.
Q2. What is the difference between Root and Sudo?

Answer:

The Root user has unrestricted access to the entire system.

sudo allows a normal user to execute specific commands with root privileges without logging in as the root user.

Using sudo is more secure because administrative access is controlled and logged.

Q3. Why shouldn't we work as the Root user all the time?

Answer:

Working as Root is risky because even a small mistake can affect the entire system.

Linux follows the Principle of Least Privilege, so administrators usually log in as normal users and use sudo only when necessary.

Q4. What is a UID?

Answer:

UID (User ID) is a unique numeric identifier assigned to every Linux user.

Linux internally identifies users by UID rather than by username.

Q5. What is a GID?

Answer:

GID (Group ID) is the unique numeric identifier assigned to every Linux group.

Linux uses GID to manage group permissions.

Q6. What is the difference between UID and GID?

Answer:

UID identifies a user.
GID identifies a group.
Q7. What is the difference between a Primary Group and a Secondary Group?

Answer:

Every user has one Primary Group.

A user can belong to multiple Secondary (Supplementary) Groups to gain additional permissions.

Q8. Why do companies use groups?

Answer:

Groups simplify permission management.

Instead of assigning permissions individually to hundreds of users, administrators assign permissions to a group and add users to that group.

This improves scalability and administration.

Q9. Why are /etc/passwd and /etc/shadow separate files?

Answer:

/etc/passwd stores user account information and is readable by all users.

/etc/shadow stores password hashes and password aging information and is readable only by the root user for security.

Q10. What information is stored in /etc/passwd?

Answer:

It stores:

Username
UID
GID
Home Directory
Login Shell
Password placeholder (x)

It does not store passwords.

Q11. What information is stored in /etc/shadow?

Answer:

It stores:

Password Hash
Last Password Change
Password Expiry
Password Aging Information
Warning Period

Only the root user can access this file.

Q12. Why does Linux store hashed passwords?

Answer:

Hashing protects passwords because the original password cannot be recovered from the stored hash.

Even if an attacker gains access to /etc/shadow, they cannot directly read users' passwords.

Q13. What is the difference between Hashing and Encryption?

Answer:

Hashing

One-way process
Cannot be reversed
Used for password storage

Encryption

Two-way process
Can be decrypted using a key
Used for protecting recoverable data
Q14. Why do service accounts use /usr/sbin/nologin?

Answer:

Service accounts are created only to run applications.

They do not require interactive login.

Using /usr/sbin/nologin improves security by preventing users from logging in using those accounts.

Q15. What is the difference between useradd and usermod?

Answer:

useradd creates a new user.
usermod modifies an existing user.
Q16. Why do we use usermod -aG instead of usermod -G?

Answer:

-aG appends the new group while preserving existing group memberships.

-G replaces all supplementary groups, which may accidentally remove important permissions.

Q17. What is the difference between userdel and userdel -r?

Answer:

userdel removes only the user account.
userdel -r removes the user account, home directory, and mail spool.
Q18. Why do companies lock user accounts before deleting them?

Answer:

Companies lock accounts first because:

Project files may still be required.
Managers or HR may need access.
The account can be restored if necessary.

After the retention period, the account can be deleted.

Q19. What is the Principle of Least Privilege (PoLP)?

Answer:

The Principle of Least Privilege means users and applications should receive only the minimum permissions required to perform their tasks.

This minimizes security risks and prevents unauthorized access.

Q20. A developer gets the error:
permission denied while trying to connect to the Docker daemon

How would you fix it?

Answer:

Instead of giving the user root access, add them to the Docker group:

sudo usermod -aG docker username

Then ask the user to log out and log back in (or start a new login session) so the new group membership takes effect.

This follows the Principle of Least Privilege.

⭐ Bonus Interview Questions (Asked in Experienced DevOps Interviews)
Q21. Why is UID 0 special?

Answer: UID 0 belongs to the Root user, which has unrestricted administrative privileges on the system.

Q22. How can you check which groups a user belongs to?

Answer:

groups username

or

id username
Q23. How do you lock and unlock a user account?

Answer:

Lock:

sudo passwd -l username

Unlock:

sudo passwd -u username
Q24. Which file stores group information?

Answer:

/etc/group
Q25. Why do applications like Jenkins run as dedicated users instead of Root?

Answer:

Running applications as dedicated system users limits their permissions. If the application is compromised, the attacker gains access only to that service account rather than full root privileges, reducing the impact of a security breach.
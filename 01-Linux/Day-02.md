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

# 7. common linux user commands 

Linux provides several commands to create, modify, and manage users. These commands are frequently used by Linux Administrators and DevOps Engineers in production environments.

Create a New User

The useradd command is used to create a new user account in Linux.

Syntax

sudo useradd username

Example

sudo useradd anil

💡 Real-Time Example

When a new employee joins a company, the Linux administrator creates a user account before providing server access.

Change Password

The passwd command is used to create or change a user's password.

Change your own password

passwd

Change another user's password (Root/Sudo)

sudo passwd username

💡 Real-Time Example

When a user forgets their password, the Linux administrator resets it using the passwd command.

Display Current User

The whoami command displays the username of the currently logged-in user.

whoami
Display User Information

The id command displays detailed information about a user.

It shows:

User ID (UID)
Group ID (GID)
Primary Group
Secondary Groups
id
Display User Groups

The groups command displays all groups to which a user belongs.

groups
Display Logged-in Users

The who command displays all users currently logged into the Linux system.

who
# 8. Modyfying existing Groups


The usermod command is used to modify an existing user account.

Change Login Shell
sudo usermod -s /bin/bash username

Changes the default login shell of a user.

Change Home Directory
sudo usermod -d /new/home -m username

The -m option moves all existing files to the new home directory.

Add User to a Group
sudo usermod -aG docker username
Why do we use -aG instead of -G?
-aG appends the user to a new group while keeping existing group memberships.
-G removes all existing supplementary groups and adds only the specified group.

⚠️ Interview Tip: Always use -aG unless you intentionally want to replace all supplementary groups.

# 9. Deleting Users

The userdel command is used to remove users from the Linux system.

Delete Only the User Account
sudo userdel username

This removes only the user account.

The user's home directory remains on the system.

Delete User Along with Home Directory
sudo userdel -r username

This removes:

User Account
Home Directory
Mail Spool

💡 Production Practice

Most companies first lock the account, retain the data for a specific period, and then permanently delete it using userdel -r.


# 10. Group Management

Linux groups simplify permission management by allowing administrators to assign permissions to groups instead of individual users.

Create a Group
sudo groupadd groupname

Example

sudo groupadd devops
Delete a Group
sudo groupdel groupname

⚠️ A group cannot be deleted if it is still the Primary Group of any user.

# 11. Important Linux Files
/etc/passwd

Stores basic information about every user account.

It contains:

Username
UID
GID
Home Directory
Login Shell

It does not store passwords.

/etc/shadow

Stores password-related information such as:

Password Hash
Password Expiry
Password Aging
Password Warning Period

Only the Root User can access this file.

/etc/group

Stores information related to Linux groups.

It contains:

Group Name
Group ID (GID)
Members of the Group
# 12. Hashing vs Encryption
Hashing
One-way process
Cannot be reversed
Used for storing passwords
More secure for authentication
Encryption
Two-way process
Can be decrypted using a key
Used for protecting sensitive data that needs to be recovered
# 13. Real-Time DevOps Scenarios
Scenario 1 – Docker Permission Denied

Instead of giving a developer Root access,

sudo usermod -aG docker username

adds the user to the Docker group.

This follows the Principle of Least Privilege.

Scenario 2 – Employee Leaves the Company

Most organizations follow these steps:

Lock the user account.
Retain project files.
Back up important data.
Delete the account after the retention period.

# 14. Interview Questions
Q1. What are the different types of users in Linux?

Answer:

Linux has three types of users:

Root User – Administrator with complete system access.
Normal User – Regular user with limited permissions.
System User – Created for services like Jenkins, MySQL, and Nginx.
Q2. What is the difference between Root User and a Normal User?

Answer:

The Root User has unrestricted access to the entire system and can perform any administrative task. A Normal User has limited permissions and requires sudo to execute administrative commands.

Q3. What is sudo?

Answer:

sudo (Super User Do) allows a normal user to execute commands with root privileges without logging in as the root user. It improves security because administrative actions are controlled and logged.

Q4. Why shouldn't we always work as the Root user?

Answer:

Working as the Root user is risky because a single incorrect command can damage the entire system. Linux follows the Principle of Least Privilege (PoLP), where users receive only the permissions necessary for their tasks.

Q5. What is a UID?

Answer:

UID (User ID) is a unique numeric identifier assigned to every Linux user. Linux internally identifies users by UID rather than by username.

Q6. What is a GID?

Answer:

GID (Group ID) is a unique numeric identifier assigned to every Linux group. It is used by Linux to manage group permissions.

Q7. What is the difference between UID and GID?

Answer:

UID identifies a user.
GID identifies a group.
Q8. What is the difference between a Primary Group and a Secondary Group?

Answer:

Every user has one Primary Group, which is assigned by default. A user can also belong to multiple Secondary (Supplementary) Groups to gain additional permissions.

Q9. Why do companies use Linux Groups?

Answer:

Groups simplify permission management. Instead of assigning permissions to individual users, administrators assign permissions to a group and add users to that group.

Q10. What is the purpose of the /etc/passwd file?

Answer:

The /etc/passwd file stores basic user account information such as:

Username
UID
GID
Home Directory
Login Shell

It does not store passwords.

Q11. What does the x in /etc/passwd represent?

Answer:

The x indicates that the actual password hash is stored in the /etc/shadow file instead of /etc/passwd.

Q12. What information is stored in /etc/shadow?

Answer:

The /etc/shadow file stores:

Password Hash
Password Aging
Password Expiry
Last Password Change
Warning Period

Only the Root user can access this file.

Q13. Why are /etc/passwd and /etc/shadow separate files?

Answer:

/etc/passwd is readable by all users because many applications need user information. Password hashes are stored separately in /etc/shadow, which only the Root user can access, improving security.

Q14. Why does Linux store hashed passwords instead of plain-text passwords?

Answer:

Hashing is a one-way process, so the original password cannot be recovered from the stored hash. This protects user credentials even if the password database is compromised.

Q15. What is the difference between Hashing and Encryption?

Answer:

Hashing

One-way process
Cannot be reversed
Used for password storage

Encryption

Two-way process
Can be decrypted using a key
Used for protecting recoverable data
Q16. What is the purpose of the useradd command?

Answer:

The useradd command creates a new user account in Linux.

Example:

sudo useradd anil
Q17. What is the purpose of the usermod command?

Answer:

The usermod command modifies an existing user account. It can change the login shell, home directory, group memberships, and other user properties.

Q18. Why do we use usermod -aG instead of usermod -G?

Answer:

-aG appends the user to a new group while preserving existing supplementary group memberships.

-G replaces all supplementary groups with the specified group.

Q19. What is the difference between userdel and userdel -r?

Answer:

userdel removes only the user account.
userdel -r removes the user account, home directory, and mail spool.
Q20. Why do companies lock user accounts before deleting them?

Answer:

Companies first lock user accounts because project files or important data may still be needed. After the retention period, the account can be permanently deleted.

Q21. What is the purpose of /etc/group?

Answer:

The /etc/group file stores:

Group Name
Group ID (GID)
Members of the Group

It is used by Linux to manage group memberships.

Q22. How can you check which groups a user belongs to?

Answer:

Using the groups command:

groups username

or

id username
Q23. How do you lock and unlock a user account?

Answer:

Lock a user account

sudo passwd -l username

Unlock a user account

sudo passwd -u username
Q24. Why do applications like Jenkins or MySQL run as dedicated system users instead of Root?

Answer:

Running applications as dedicated system users limits their permissions. If the application is compromised, the attacker gains access only to that service account instead of full root privileges, improving security.

Q25. A developer gets the following error:
permission denied while trying to connect to the Docker daemon

How would you fix it?

Answer:

Instead of giving the developer Root access, add the user to the Docker group:

sudo usermod -aG docker username

Then ask the user to log out and log back in (or start a new login session) so the new group membership takes effect.

This follows the Principle of Least Privilege (PoLP).
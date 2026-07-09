# 📅 Day 06 - Shell Scripting 

# 1️⃣ Introduction to Shell Scripting

## What is Shell?

A **Shell** is a command-line interpreter that acts as an interface between the user and the Linux operating system. It accepts user commands, passes them to the Linux kernel, and displays the output.

In simple words:

```
User → Shell → Kernel → Hardware
```

The shell allows users to interact with the operating system by executing commands.

---

## What is Shell Scripting?

A **Shell Script** is a text file that contains a sequence of Linux commands executed one after another by the shell.

Instead of typing commands manually every time, we can automate them by writing them into a script.

Example:

```bash
#!/bin/bash

echo "Welcome to DevOps"
date
pwd
```

Save it as:

```
welcome.sh
```

Run:

```bash
./welcome.sh
```

Output

```
Welcome to DevOps

Thu Jul 9

/home/ubuntu
```

---

# Why Shell Scripting?

Shell scripting helps automate repetitive tasks.

Instead of executing 20 commands every day, we can execute one script.

---

# Advantages

- Automation
- Saves time
- Reduces manual work
- Reduces human errors
- Easy to maintain
- Easy to reuse
- Widely used in DevOps
- Improves productivity

---

# Real-Time DevOps Uses

DevOps Engineers use Shell Scripts for:

- Deploying applications
- Restarting services
- Creating backups
- Monitoring servers
- Docker automation
- Kubernetes automation
- Jenkins pipelines
- AWS automation
- Log cleanup
- Health checks

---

# 2️⃣ Shell vs Shell Script

| Shell | Shell Script |
|--------|--------------|
| Command Interpreter | Collection of commands |
| Executes one command at a time | Executes multiple commands |
| Interactive | Automated |
| Manual execution | Scheduled or automatic execution |

---

# 3️⃣ Types of Shell

Linux provides different shells.

## Bash

Bourne Again Shell

Most popular shell.

Default in Ubuntu.

---

## Sh

Bourne Shell

Old shell.

---

## Ksh

Korn Shell

Improved version of Bourne Shell.

---

## Csh

C Shell

Uses C language syntax.

---

## Tcsh

Enhanced C Shell.

---

## Zsh

Modern shell.

Offers better customization and auto-completion.

---

# Which Shell is Mostly Used?

Bash.

Almost every DevOps Engineer uses Bash for automation.

---

# Check Current Shell

```bash
echo $SHELL
```

Example

```
/bin/bash
```

---

# List Available Shells

```bash
cat /etc/shells
```

---

# 4️⃣ Shebang

## What is Shebang?

The first line of a shell script.

Example

```bash
#!/bin/bash
```

It tells Linux which interpreter should execute the script.

Without the correct interpreter, the script may not run as expected.

---

# Common Shebangs

Bash

```bash
#!/bin/bash
```

Sh

```bash
#!/bin/sh
```

Python

```python
#!/usr/bin/python3
```

---

# 5️⃣ Creating a Shell Script

Create a file

```bash
touch hello.sh
```

Open

```bash
vi hello.sh
```

or

```bash
nano hello.sh
```

Write

```bash
#!/bin/bash

echo "Hello World"
```

Save the file.

---

# Make Script Executable

```bash
chmod +x hello.sh
```

---

# Run Script

Method 1

```bash
./hello.sh
```

Method 2

```bash
bash hello.sh
```

Method 3

```bash
sh hello.sh
```

---

# Difference Between These Methods

```
./hello.sh
```

Requires execute permission.

Uses the interpreter specified in the shebang.

---

```
bash hello.sh
```

Does not require execute permission.

Always uses Bash.

---

```
sh hello.sh
```

Runs using Bourne Shell.

Some Bash-specific features may not work.

---

# 6️⃣ Variables

## What is a Variable?

A variable is a named memory location used to store data.

Example

```bash
name="Sahithi"
```

---

# Access Variable

```bash
echo $name
```

Output

```
Sahithi
```

---

# Rules

- Starts with letter or underscore
- Cannot start with number
- No spaces
- Case-sensitive

Valid

```bash
name
```

```bash
_name
```

```bash
age1
```

Invalid

```bash
1name
```

```bash
my name
```

---

# Multiple Variables

```bash
name="Sahithi"

course="DevOps"

city="Hyderabad"
```

---

# Command Output into Variable

```bash
today=$(date)
```

```bash
current_dir=$(pwd)
```

---

# Read-only Variable

Used when the value should never change.

```bash
readonly company="OpenAI"
```

Trying to modify it results in an error.

---

# Remove Variable

```bash
unset name
```

After:

```bash
echo $name
```

Output

```
(blank)
```

---

# Environment Variables

Environment variables are available to child processes and other applications.

Common examples

```bash
HOME
```

```bash
PATH
```

```bash
USER
```

```bash
SHELL
```

Display

```bash
echo $HOME
```

---

# export

Makes a variable available to child processes.

Example

```bash
export APP_ENV=production
```

Now child scripts can access:

```bash
echo $APP_ENV
```

---

# Local Variable vs Environment Variable

| Local Variable | Environment Variable |
|---------------|----------------------|
| Current shell only | Available to child processes |
| Not exported | Exported using `export` |

---

# DevOps Real-Time Example

```bash
export JAVA_HOME=/usr/lib/jvm/java-17

export PATH=$PATH:$JAVA_HOME/bin
```

Many DevOps tools like Java, Maven, Terraform, AWS CLI, and Kubernetes rely on environment variables.

---

# Best Practices

- Use meaningful variable names.
- Avoid hardcoding values.
- Store reusable values in variables.
- Use environment variables for configuration.
- Use `readonly` for constants.

---

# ⭐ Quick Revision

| Command | Purpose |
|----------|---------|
| `echo $SHELL` | Current shell |
| `cat /etc/shells` | List available shells |
| `chmod +x file.sh` | Make script executable |
| `./file.sh` | Run script |
| `bash file.sh` | Run using Bash |
| `name="Sahithi"` | Create variable |
| `echo $name` | Print variable |
| `unset name` | Remove variable |
| `readonly name="DevOps"` | Constant variable |
| `export VAR=value` | Environment variable |

---

# 🎯 Interview Questions

## 1. What is Shell Scripting?

A shell script is a text file containing Linux commands that are executed sequentially by the shell. It is used to automate repetitive tasks.

---

## 2. Difference between Shell and Shell Script?

- Shell is a command interpreter.
- Shell Script is a file containing multiple shell commands.

---

## 3. Which shell is mostly used in DevOps?

Bash (Bourne Again Shell).

---

## 4. What is Shebang?

The first line of a script that specifies which interpreter should execute it.

Example:

```bash
#!/bin/bash
```

---

## 5. How do you make a shell script executable?

```bash
chmod +x script.sh
```

---

## 6. Difference between `./script.sh` and `bash script.sh`?

- `./script.sh` uses the shebang and requires execute permission.
- `bash script.sh` runs the script with Bash and does not require execute permission.

---

## 7. What is a variable?

A variable stores a value that can be used later in the script.

---

## 8. What is an environment variable?

An environment variable is available to the current shell and its child processes.

---

## 9. Why do we use `export`?

To make a variable available to child processes.

---

## 10. Difference between `unset` and `readonly`?

- `unset` removes a variable.
- `readonly` makes a variable immutable so it cannot be modified.

---

# 📅 Day 06 - Shell Scripting (Part 2)

# 📌 Topics Covered

- User Input
- Arithmetic Operators
- Relational Operators
- Logical Operators
- String Operators
- File Test Operators

---

# 7️⃣ User Input

## What is User Input?

User input allows a shell script to accept values from the user while the script is running.

Instead of hardcoding values inside the script, the user can provide them at runtime, making scripts more flexible and reusable.

---

## Why Do DevOps Engineers Use User Input?

DevOps Engineers use user input to:

- Select deployment environments (Dev, QA, Prod)
- Enter Docker image tags
- Specify server names
- Choose Kubernetes namespaces
- Provide backup locations
- Accept usernames and passwords

---

## Syntax

```bash
read variable_name
```

---

## Example

```bash
#!/bin/bash

echo "Enter your name"

read name

echo "Welcome $name"
```

Output

```
Enter your name

Sahithi

Welcome Sahithi
```

---

## Prompt Message

Instead of using echo:

```bash
read -p "Enter your name: " name
```

Output

```
Enter your name: Sahithi
```

---

## Read Multiple Values

```bash
read first last
```

Example

```bash
#!/bin/bash

echo "Enter First and Last Name"

read first last

echo "First Name: $first"

echo "Last Name: $last"
```

---

## Hidden Input

Used for passwords.

```bash
read -s password
```

Example

```bash
read -s -p "Enter Password: " password
```

Characters are not displayed while typing.

---

## Read Array Input

```bash
read -a names
```

Example

```
Ram Ravi Sahithi
```

---

## DevOps Example

```bash
read -p "Enter Environment: " env

echo "Deploying to $env"
```

---

# 8️⃣ Operators

Operators perform calculations, comparisons, and logical operations.

Types of Operators:

- Arithmetic
- Relational
- Logical
- String
- File Test

---

# Arithmetic Operators

Used for mathematical calculations.

| Operator | Meaning |
|----------|----------|
| + | Addition |
| - | Subtraction |
| * | Multiplication |
| / | Division |
| % | Modulus |

---

## Example

```bash
a=20

b=10

echo $((a+b))

echo $((a-b))

echo $((a*b))

echo $((a/b))

echo $((a%b))
```

Output

```
30

10

200

2

0
```

---

# Relational Operators

Used for comparing numbers.

| Operator | Meaning |
|----------|----------|
| -eq | Equal |
| -ne | Not Equal |
| -gt | Greater Than |
| -lt | Less Than |
| -ge | Greater Than or Equal |
| -le | Less Than or Equal |

---

## Example

```bash
if [ $a -gt $b ]
then
echo "a is greater"
fi
```

---

# Logical Operators

Used to combine multiple conditions.

| Operator | Meaning |
|----------|----------|
| && | AND |
| || | OR |
| ! | NOT |

---

## AND Example

```bash
if [ $age -gt 18 ] && [ $salary -gt 30000 ]
then
echo "Eligible"
fi
```

Both conditions must be true.

---

## OR Example

```bash
if [ $marks -gt 90 ] || [ $sports = yes ]
then
echo "Selected"
fi
```

At least one condition must be true.

---

## NOT Example

```bash
if [ ! -f file.txt ]
then
echo "File Not Found"
fi
```

---

# String Operators

Used to compare strings.

| Operator | Meaning |
|----------|----------|
| = | Equal |
| != | Not Equal |
| -z | Empty String |
| -n | Non-empty String |

---

## Example

```bash
name="Docker"

if [ "$name" = "Docker" ]
then
echo "Correct"
fi
```

---

## Empty String

```bash
if [ -z "$name" ]
then
echo "Empty"
fi
```

---

## Non-empty String

```bash
if [ -n "$name" ]
then
echo "Available"
fi
```

---

# File Test Operators

Used to check files and directories.

| Operator | Meaning |
|----------|----------|
| -f | File exists |
| -d | Directory exists |
| -r | Read permission |
| -w | Write permission |
| -x | Execute permission |
| -e | File or Directory exists |

---

## Check File Exists

```bash
if [ -f demo.txt ]
then
echo "File Exists"
fi
```

---

## Check Directory

```bash
if [ -d backup ]
then
echo "Directory Exists"
fi
```

---

## Check Execute Permission

```bash
if [ -x script.sh ]
then
echo "Executable"
fi
```

---

# DevOps Real-Time Examples

## Check Deployment File

```bash
if [ -f deployment.yaml ]
then
kubectl apply -f deployment.yaml
fi
```

---

## Check Docker Installation

```bash
if docker --version
then
echo "Docker Installed"
fi
```

---

## Check Backup Directory

```bash
if [ -d /backup ]
then
echo "Backup Directory Found"
fi
```

---

# Best Practices

- Use quotes around string variables.
- Validate user input before processing.
- Use meaningful variable names.
- Use file operators before accessing files.
- Use logical operators to simplify conditions.

---

# ⭐ Quick Revision

| Command | Purpose |
|----------|----------|
| `read name` | Read input |
| `read -p` | Prompt input |
| `read -s` | Hidden input |
| `read -a` | Read array |
| `+ - * / %` | Arithmetic |
| `-eq` | Equal |
| `-gt` | Greater Than |
| `-lt` | Less Than |
| `&&` | AND |
| `||` | OR |
| `!` | NOT |
| `=` | String Equal |
| `!=` | String Not Equal |
| `-z` | Empty String |
| `-n` | Non-empty String |
| `-f` | File Exists |
| `-d` | Directory Exists |
| `-r` | Read Permission |
| `-w` | Write Permission |
| `-x` | Execute Permission |

---

# 🎯 Interview Questions

## 1. How do you read user input?

```bash
read variable
```

---

## 2. How do you hide password input?

```bash
read -s password
```

---

## 3. Difference between `read` and `read -p`?

- `read` waits for input.
- `read -p` displays a prompt message before accepting input.

---

## 4. Which operator checks whether two integers are equal?

```bash
-eq
```

---

## 5. Difference between `=` and `-eq`?

- `=` compares strings.
- `-eq` compares integers.

---

## 6. Difference between `&&` and `||`?

- `&&` executes when both conditions are true.
- `||` executes when at least one condition is true.

---

## 7. What does `-f` check?

Checks whether a regular file exists.

---

## 8. What does `-d` check?

Checks whether a directory exists.

---

## 9. Difference between `-z` and `-n`?

- `-z` checks whether a string is empty.
- `-n` checks whether a string is not empty.

---

## 10. Give one real-world DevOps use case for file operators.

Before deploying to Kubernetes, a script checks whether `deployment.yaml` exists using `-f`. If the file is missing, the deployment is stopped to prevent errors.

---

# 9️⃣ Conditional Statements

## What are Conditional Statements?

Conditional statements allow a script to make decisions based on whether a condition is true or false.

Instead of executing every command, the script executes only the commands that satisfy the given condition.

In DevOps, conditions are commonly used to:

- Check whether a service is running.
- Verify if a file exists.
- Check disk usage.
- Deploy to different environments.
- Validate user input.
- Monitor servers.

---

# Types of Conditional Statements

- if
- if-else
- if-elif-else
- Nested if

---

# 1. if Statement

## What is if?

The **if statement** executes commands only when the specified condition is true.

### Syntax

```bash
if [ condition ]
then
    commands
fi
```

---

### Example

```bash
#!/bin/bash

age=20

if [ $age -ge 18 ]
then
    echo "Eligible to Vote"
fi
```

Output

```
Eligible to Vote
```

---

### Example - File Check

```bash
#!/bin/bash

if [ -f backup.sh ]
then
    echo "Backup Script Exists"
fi
```

---

# 2. if-else Statement

## What is if-else?

Executes one block if the condition is true and another block if the condition is false.

### Syntax

```bash
if [ condition ]
then
    commands
else
    commands
fi
```

---

### Example

```bash
#!/bin/bash

marks=40

if [ $marks -ge 35 ]
then
    echo "Pass"
else
    echo "Fail"
fi
```

Output

```
Pass
```

---

### Example

```bash
#!/bin/bash

if [ -d backup ]
then
    echo "Directory Exists"
else
    echo "Directory Not Found"
fi
```

---

# 3. if-elif-else Statement

## What is if-elif?

Used when multiple conditions need to be checked.

Instead of writing many separate if statements, use if-elif.

### Syntax

```bash
if [ condition ]
then
    commands

elif [ condition ]
then
    commands

else
    commands
fi
```

---

### Example

```bash
#!/bin/bash

marks=82

if [ $marks -ge 90 ]
then
    echo "Grade A"

elif [ $marks -ge 75 ]
then
    echo "Grade B"

elif [ $marks -ge 60 ]
then
    echo "Grade C"

else
    echo "Fail"
fi
```

Output

```
Grade B
```

---

# DevOps Example

```bash
#!/bin/bash

env="qa"

if [ "$env" = "dev" ]
then
    echo "Deploying to Development"

elif [ "$env" = "qa" ]
then
    echo "Deploying to QA"

elif [ "$env" = "prod" ]
then
    echo "Deploying to Production"

else
    echo "Invalid Environment"
fi
```

---

# 4. Nested if

## What is Nested if?

A Nested if is an if statement inside another if statement.

Used when a second condition should be checked only if the first condition is true.

---

### Syntax

```bash
if [ condition ]
then

    if [ condition ]
    then
        commands
    fi

fi
```

---

### Example

```bash
#!/bin/bash

user="admin"

password="1234"

if [ "$user" = "admin" ]
then

    if [ "$password" = "1234" ]
    then
        echo "Login Successful"

    else
        echo "Wrong Password"

    fi

else
    echo "Invalid User"

fi
```

Output

```
Login Successful
```

---

# Real-Time DevOps Example 1

## Check Docker Installation

```bash
#!/bin/bash

if docker --version > /dev/null 2>&1
then
    echo "Docker Installed"

else
    echo "Docker Not Installed"

fi
```

---

# Real-Time DevOps Example 2

## Check Service Status

```bash
#!/bin/bash

if systemctl is-active --quiet docker
then
    echo "Docker Service Running"

else
    echo "Docker Service Stopped"

fi
```

---

# Real-Time DevOps Example 3

## Check Disk Usage

```bash
#!/bin/bash

usage=85

if [ $usage -gt 80 ]
then
    echo "Disk Usage High"

else
    echo "Disk Usage Normal"

fi
```

---

# Real-Time DevOps Example 4

## Deploy to Different Environments

```bash
#!/bin/bash

env="prod"

if [ "$env" = "dev" ]
then
    echo "Deploying to Development"

elif [ "$env" = "qa" ]
then
    echo "Deploying to QA"

elif [ "$env" = "prod" ]
then
    echo "Deploying to Production"

else
    echo "Invalid Environment"

fi
```

---

# Best Practices

- Keep conditions simple.
- Use quotes while comparing strings.
- Use meaningful variable names.
- Check file existence before accessing files.
- Use if-elif instead of multiple independent if statements when checking related conditions.

---

# ⭐ Quick Revision

| Statement | Purpose |
|-----------|----------|
| if | Execute when condition is true |
| if-else | Execute one of two blocks |
| if-elif-else | Check multiple conditions |
| Nested if | if inside another if |

---

# 🎯 Interview Questions

## 1. What is a conditional statement?

A conditional statement allows a script to make decisions by executing commands based on whether a condition is true or false.

---

## 2. What is the difference between if and if-else?

- **if** executes commands only when the condition is true.
- **if-else** executes one block when the condition is true and another block when it is false.

---

## 3. When do you use if-elif-else?

When multiple conditions need to be checked, such as deploying to Dev, QA, or Production environments.

---

## 4. What is Nested if?

Nested if is an if statement placed inside another if statement. It is used when a second condition depends on the first condition being true.

---

## 5. Give one real-time DevOps use case of if statements.

A deployment script checks whether `deployment.yaml` exists before running:

```bash
if [ -f deployment.yaml ]
then
    kubectl apply -f deployment.yaml
fi
```

This prevents deployment failures if the file is missing.

---

## 6. Why should string variables be enclosed in quotes?

Quotes prevent errors when variables are empty or contain spaces.

Example:

```bash
if [ "$env" = "prod" ]
```

---

## 7. Why are conditional statements important in DevOps?

Conditional statements help automate decisions such as checking service status, validating files, selecting deployment environments, monitoring servers, and handling errors.

---

# 📅 Day 06 - Shell Scripting (Part 4)

# 📌 Topics Covered

- for Loop
- while Loop
- until Loop
- break Statement
- continue Statement
- Real-Time DevOps Examples

---

# 🔟 Loops

## What are Loops?

A loop is used to execute the same block of code repeatedly until a condition becomes false.

Instead of writing the same commands multiple times, loops execute them automatically.

---

## Why Do DevOps Engineers Use Loops?

DevOps Engineers use loops to:

- Restart multiple servers
- Deploy applications to multiple environments
- Check multiple services
- Process log files
- Monitor multiple servers
- Backup multiple directories
- Create multiple users
- Delete old files

Loops reduce duplicate code and make automation easier.

---

# Types of Loops

- for Loop
- while Loop
- until Loop

---

# 1️⃣ for Loop

## What is a for Loop?

A **for loop** is used when the number of iterations is known.

It executes commands once for each item in a list.

---

## Syntax

```bash
for variable in list
do
    commands
done
```

---

## Example

```bash
#!/bin/bash

for i in 1 2 3 4 5
do
    echo $i
done
```

Output

```
1
2
3
4
5
```

---

## Using Range

```bash
for i in {1..5}
do
    echo $i
done
```

---

## Loop Through Array

```bash
servers=("server1" "server2" "server3")

for server in "${servers[@]}"
do
    echo $server
done
```

Output

```
server1
server2
server3
```

---

# 2️⃣ while Loop

## What is a while Loop?

A **while loop** executes commands repeatedly as long as the condition is true.

Used when the number of iterations is unknown.

---

## Syntax

```bash
while [ condition ]
do
    commands
done
```

---

## Example

```bash
#!/bin/bash

count=1

while [ $count -le 5 ]
do
    echo $count
    count=$((count+1))
done
```

Output

```
1
2
3
4
5
```

---

## Real-Time Example

Monitor a service continuously.

```bash
while true
do
    date
    sleep 5
done
```

Prints the current date every 5 seconds.

---

# 3️⃣ until Loop

## What is an until Loop?

An **until loop** executes commands until the condition becomes true.

Unlike a while loop, it continues while the condition is false.

---

## Syntax

```bash
until [ condition ]
do
    commands
done
```

---

## Example

```bash
#!/bin/bash

count=1

until [ $count -gt 5 ]
do
    echo $count
    count=$((count+1))
done
```

Output

```
1
2
3
4
5
```

---

# Difference Between while and until

| while | until |
|--------|--------|
| Runs while condition is true | Runs until condition becomes true |
| Stops when condition becomes false | Stops when condition becomes true |

---

# 4️⃣ break Statement

## What is break?

The **break** statement immediately terminates the loop.

---

## Example

```bash
for i in {1..10}
do

    if [ $i -eq 5 ]
    then
        break
    fi

    echo $i

done
```

Output

```
1
2
3
4
```

The loop stops when `i` becomes 5.

---

# Real-Time DevOps Example

Stop checking servers once an unhealthy server is found.

```bash
for server in server1 server2 server3
do
    if [ "$server" = "server2" ]
    then
        echo "Server Down"
        break
    fi

    echo "$server is Healthy"
done
```

---

# 5️⃣ continue Statement

## What is continue?

The **continue** statement skips the current iteration and moves to the next iteration.

---

## Example

```bash
for i in {1..5}
do

    if [ $i -eq 3 ]
    then
        continue
    fi

    echo $i

done
```

Output

```
1
2
4
5
```

Number 3 is skipped.

---

# Real-Time DevOps Example

Skip a server under maintenance.

```bash
servers=("server1" "server2" "server3")

for server in "${servers[@]}"
do

    if [ "$server" = "server2" ]
    then
        continue
    fi

    echo "Checking $server"

done
```

Output

```
Checking server1

Checking server3
```

---

# Real-Time DevOps Examples

## Deploy to Multiple Servers

```bash
servers=("web1" "web2" "web3")

for server in "${servers[@]}"
do
    echo "Deploying application to $server"
done
```

---

## Restart Multiple Services

```bash
services=("docker" "nginx" "jenkins")

for service in "${services[@]}"
do
    sudo systemctl restart $service
done
```

---

## Backup Multiple Directories

```bash
folders=("/home" "/var/log" "/etc")

for folder in "${folders[@]}"
do
    echo "Backing up $folder"
done
```

---

## Check Multiple Pods

```bash
pods=$(kubectl get pods --no-headers | awk '{print $1}')

for pod in $pods
do
    echo "Checking $pod"
done
```

---

## Monitor CPU Continuously

```bash
while true
do
    top -bn1 | head -5
    sleep 10
done
```

---

# Best Practices

- Use **for loops** when the number of iterations is known.
- Use **while loops** for continuous monitoring.
- Avoid infinite loops unless necessary.
- Use **break** to exit early when required.
- Use **continue** to skip unnecessary iterations.
- Keep loop logic simple and readable.

---

# ⭐ Quick Revision

| Loop | Purpose |
|------|----------|
| for | Known number of iterations |
| while | Executes while condition is true |
| until | Executes until condition becomes true |
| break | Exit loop immediately |
| continue | Skip current iteration |

---

# 🎯 Interview Questions

## 1. What is a loop?

A loop repeatedly executes a block of code until a specified condition is met.

---

## 2. What are the different types of loops in Shell Scripting?

- for loop
- while loop
- until loop

---

## 3. When do you use a for loop?

A for loop is used when the number of iterations is known in advance.

---

## 4. When do you use a while loop?

A while loop is used when the loop should continue as long as a condition remains true, such as continuous monitoring.

---

## 5. What is the difference between while and until?

- **while** executes while the condition is true.
- **until** executes until the condition becomes true.

---

## 6. What is break?

`break` immediately terminates the current loop.

---

## 7. What is continue?

`continue` skips the current iteration and moves to the next iteration.

---

## 8. Give one real-time DevOps use case for a for loop.

A DevOps Engineer can restart multiple services using a for loop.

Example:

```bash
for service in docker nginx jenkins
do
    sudo systemctl restart $service
done
```

---

## 9. Give one real-time DevOps use case for a while loop.

A while loop can continuously monitor CPU usage or application health until the script is stopped.

---

## 10. Why are loops important in DevOps?

Loops help automate repetitive tasks such as deployments, backups, health checks, server management, service restarts, and Kubernetes operations, reducing manual effort and improving consistency.

---


# 1️⃣ Functions

## What is a Function?

A function is a reusable block of code that performs a specific task. Instead of writing the same commands multiple times, we write them once inside a function and call the function whenever needed.

Functions improve:
- Code reusability
- Readability
- Maintainability
- Modularity

### Syntax

```bash
function_name() {
    commands
}
```

or

```bash
function function_name {
    commands
}
```

### Example

```bash
#!/bin/bash

greet() {
    echo "Welcome to DevOps"
}

greet
```

### Passing Arguments

Arguments passed to functions are accessed using:

- `$1` → First argument
- `$2` → Second argument
- `$3` → Third argument

Example:

```bash
greet() {
    echo "Welcome $1"
}

greet Sahithi
```

### Local Variables

Variables inside functions can be made local.

```bash
local name="Docker"
```

### Return

Functions return exit status using:

```bash
return 0
```

To return text, use:

```bash
echo "Hello"
```

and capture it:

```bash
result=$(function_name)
```

### Real-Time DevOps Uses

- Restart services
- Deploy applications
- Health checks
- Logging
- Backups

---

# 2️⃣ Command-Line Arguments

## What are Command-Line Arguments?

Command-line arguments are values passed to a script during execution.

Example

```bash
./deploy.sh dev v1
```

Here:

- dev → First argument
- v1 → Second argument

---

## Positional Parameters

| Variable | Meaning |
|----------|---------|
| `$0` | Script Name |
| `$1` | First Argument |
| `$2` | Second Argument |
| `$3` | Third Argument |

Example

```bash
echo $0
echo $1
echo $2
```

---

## `$#`

Returns the total number of arguments.

```bash
echo $#
```

---

## `$@`

Returns all arguments separately.

Example

```bash
for arg in "$@"
do
echo $arg
done
```

---

## `$*`

Returns all arguments as one string.

```bash
echo "$*"
```

---

## `$$`

Returns PID of current script.

```bash
echo $$
```

---

## `$!`

Returns PID of last background process.

```bash
sleep 100 &
echo $!
```

---

## shift

Moves arguments left.

Before

```
$1=dev
$2=v1
```

After shift

```
$1=v1
```

---

## DevOps Uses

- Environment selection
- Docker image versions
- Namespace selection
- Deployment scripts

---

# 3️⃣ Case Statement

## What is Case?

Case is used when multiple fixed values need to be checked.

Instead of writing many if-elif statements, we use case.

### Syntax

```bash
case variable in

value1)
commands
;;

value2)
commands
;;

*)
default
;;

esac
```

---

### Example

```bash
case $env in

dev)
echo "Deploy Dev"
;;

qa)
echo "Deploy QA"
;;

prod)
echo "Deploy Prod"
;;

*)
echo "Invalid"
;;

esac
```

---

### Multiple Values

```bash
start|run)
echo "Application Started"
;;
```

---

### Wildcard

```
*
```

acts like else.

---

## DevOps Uses

- Environment selection
- Service management
- Backup scheduling
- Deployment scripts

---

# 4️⃣ Arrays

## What is an Array?

Array stores multiple values inside one variable.

Example

```bash
servers=("server1" "server2" "server3")
```

---

## Access Elements

```bash
${servers[0]}
```

---

## Display All

```bash
echo "${servers[@]}"
```

---

## Number of Elements

```bash
echo ${#servers[@]}
```

---

## Length of One Element

```bash
echo ${#servers[0]}
```

---

## Loop Through Array

```bash
for server in "${servers[@]}"
do
echo $server
done
```

---

## Add Element

```bash
servers+=("server4")
```

---

## Update Element

```bash
servers[1]="production"
```

---

## Delete Element

```bash
unset servers[1]
```

---

## Read User Input

```bash
read -a names
```

---

## DevOps Uses

- Servers
- Containers
- Namespaces
- Services
- IP Addresses

---

# 5️⃣ String Operations

## String Length

```bash
${#name}
```

---

## Concatenation

```bash
result=$first$second
```

---

## Substring

```bash
${text:start:length}
```

Example

```bash
${text:0:6}
```

---

## Replace

```bash
${name/Docker/Kubernetes}
```

---

## Replace All

```bash
${text//dev/prod}
```

---

## Uppercase

```bash
${name^^}
```

---

## Lowercase

```bash
${name,,}
```

---

## Empty String

```bash
-z
```

---

## Non-empty String

```bash
-n
```

---

## Compare Strings

```bash
if [ "$env" = "prod" ]
```

---

## DevOps Uses

- Docker image tags
- Backup names
- Environment names
- Version extraction
- File naming

---

# 6️⃣ Script Debugging

## What is Debugging?

Debugging is the process of finding and fixing errors in scripts.

---

## Debug Entire Script

```bash
bash -x script.sh
```

Shows every command before execution.

---

## Enable Debugging

```bash
set -x
```

---

## Disable Debugging

```bash
set +x
```

---

## Common Errors

- Wrong variable
- Wrong command
- Wrong path
- Wrong condition

---

## DevOps Uses

- Debug CI/CD Pipelines
- Debug Docker Builds
- Debug Kubernetes Deployments

---

# 7️⃣ Error Handling

## What is Error Handling?

Handling failures properly so scripts don't continue after critical errors.

---

## Exit Status

```
0 → Success

Non-zero → Failure
```

Check exit status

```bash
echo $?
```

---

## Exit

```bash
exit 0

exit 1
```

---

## set -e

Stops script immediately if any command fails.

```bash
set -e
```

---

## &&

Runs next command only if previous command succeeds.

```bash
mkdir demo && echo "Success"
```

---

## ||

Runs next command only if previous command fails.

```bash
mkdir demo || echo "Failed"
```

---

## trap

Runs commands when signals occur.

Example

```bash
trap "echo Interrupted" SIGINT
```

---

## DevOps Uses

- Stop failed deployments
- Handle Docker build failures
- Prevent broken Kubernetes deployments

---

# 8️⃣ Cron Jobs

## What is Cron?

Cron is a Linux scheduler used to execute commands or scripts automatically at specified times.

---

## Why Cron?

Used for:

- Backups
- Health Checks
- Log Cleanup
- Monitoring
- Service Restart
- Reports

---

## Commands

Edit cron jobs

```bash
crontab -e
```

List jobs

```bash
crontab -l
```

Delete jobs

```bash
crontab -r
```

---

## Cron Format

```
* * * * *

| | | | |

| | | | Day of Week

| | | Month

| | Day

| Hour

Minute
```

---

## Examples

Every minute

```bash
* * * * *
```

Every 5 minutes

```bash
*/5 * * * *
```

Every day at 2 AM

```bash
0 2 * * *
```

Every Sunday

```bash
0 0 * * 0
```

---

## Redirect Output

```bash
0 2 * * * /home/ubuntu/backup.sh >> /var/log/backup.log 2>&1
```

---

## Best Practices

- Use absolute paths
- Redirect logs
- Test scripts before scheduling
- Keep cron jobs simple
- Monitor logs regularly

---

# ⭐ Key Commands Summary

| Command | Purpose |
|----------|----------|
| `bash -x script.sh` | Debug entire script |
| `set -x` | Enable debugging |
| `set +x` | Disable debugging |
| `set -e` | Stop on first error |
| `exit` | Exit script |
| `trap` | Handle signals |
| `crontab -e` | Edit cron jobs |
| `crontab -l` | List cron jobs |
| `crontab -r` | Remove cron jobs |
| `${#var}` | String length |
| `${array[@]}` | All array elements |
| `${#array[@]}` | Number of array elements |
| `shift` | Shift command-line arguments |
| `$?` | Exit status |
| `$$` | Current script PID |
| `$!` | Last background process PID |

---

# 📌 DevOps Real-Time Use Cases

- Functions for deployment, backups, and health checks.
- Command-line arguments for environment selection.
- Case statements for deployment environments.
- Arrays for managing multiple servers and containers.
- String operations for Docker image tags and filenames.
- Debugging failed CI/CD pipelines.
- Error handling to stop deployments after failures.
- Cron jobs for backups, monitoring, and log cleanup.

---


# Shell Scripting Interview Questions

### 1. What is Shell Scripting?

Answer:

A shell script is a text file containing Linux commands that are executed sequentially by the shell. It is used to automate repetitive tasks such as deployments, backups, monitoring, and system administration.

### 2. What is the difference between Shell and Shell Script?

Answer:

Shell: A command-line interpreter that accepts and executes commands.
Shell Script: A file containing multiple shell commands executed as a program.
### 3. What are the different types of Shells?

Answer:

Bash (Bourne Again Shell)
Sh (Bourne Shell)
Ksh (Korn Shell)
Csh (C Shell)
Tcsh
Zsh

Bash is the most commonly used shell in Linux.

### 4. What is the shebang (#!/bin/bash)?

Answer:

It tells the operating system which interpreter should execute the script. #!/bin/bash specifies that the script should run using the Bash shell.

### 5. How do you make a shell script executable?

Answer:

chmod +x script.sh

Run it using:

./script.sh
Variables
### 6. How do you create a variable?
name="Sahithi"

Access it using:

echo $name
### 7. What is the difference between local and environment variables?

Answer:

Local variables are available only in the current shell.
Environment variables are available to child processes as well after using export.
### 8. How do you export a variable?
export JAVA_HOME=/usr/lib/jvm/java-17
User Input
### 9. How do you read user input?
read name
### 10. How do you read a password securely?
read -s password
Conditions
### 11. Difference between if and case?

Answer:

if is used for conditions and comparisons.
case is used to match one value against multiple fixed options.
### 12. Difference between = and -eq?

Answer:

= compares strings.
-eq compares integers.
### 13. What is the difference between -z and -n?

Answer:

-z checks if a string is empty.
-n checks if a string is not empty.
Loops
### 14. What are the loops in Shell Scripting?

Answer:

for
while
until
### 15. Difference between for and while?

Answer:

for is used when the number of iterations is known.
while is used when the loop continues until a condition becomes false.
Functions
### 16. What is a function?

Answer:

A reusable block of code that performs a specific task and helps avoid code duplication.

### 17. How do you pass arguments to a function?
function_name arg1 arg2

Access them using:

$1
$2
Command-Line Arguments
### 18. What is $0?

Answer:

Script name.

### 19. What is $1?

Answer:

First command-line argument.

### 20. What is $#?

Answer:

Total number of command-line arguments.

### 21. Difference between $@ and $*?

Answer:

"$@" treats each argument separately.
"$*" treats all arguments as a single string when quoted.
### 22. What is shift?

Answer:

Removes the first positional parameter and shifts the remaining arguments left.

Arrays
### 23. What is an array?

Answer:

An array stores multiple values under one variable.

### 24. How do you print all array elements?
echo "${array[@]}"
### 25. How do you find the number of array elements?
echo ${#array[@]}
String Operations
### 26. How do you find the length of a string?
${#string}
### 27. How do you convert a string to uppercase?
${string^^}
### 28. How do you convert a string to lowercase?
${string,,}
### 29. How do you extract a substring?
${string:start:length}
Process & Debugging
### 30. How do you debug a script?
bash -x script.sh

or

set -x
### 31. How do you stop debugging?
set +x
Error Handling
### 32. What is an exit status?

Answer:

Every Linux command returns an exit code.

0 → Success
Non-zero → Failure
### 33. What does $? represent?

Answer:

The exit status of the last executed command.

### 34. What is set -e?

Answer:

Stops the script immediately if any command fails.

### 35. Difference between && and ||?

Answer:

&& executes the next command only if the previous succeeds.
|| executes the next command only if the previous fails.
### 36. What is trap?

Answer:

Executes specified commands when a signal or event occurs, often used for cleanup before a script exits.

Cron Jobs
### 37. What is Cron?

Answer:

Cron is a Linux scheduler used to execute commands or scripts automatically at specified times.

### 38. How do you edit cron jobs?
crontab -e
### 39. How do you list cron jobs?
crontab -l
### 40. Write a cron expression to run every 5 minutes.
*/5 * * * *
Scenario-Based Questions (Most Important)
### 41. Your deployment script fails after git pull, but still executes docker build. How do you prevent this?

Answer:

Use set -e so the script exits immediately when git pull fails.

### 42. How do you deploy to Dev, QA, and Prod using one script?

Answer:

Use command-line arguments with a case statement.

Example:

./deploy.sh dev
./deploy.sh qa
./deploy.sh prod
### 43. How would you restart multiple services using one script?

Answer:

Store service names in an array and iterate using a for loop.

### 44. How do you schedule daily backups?

Answer:

Use Cron.

Example:

0 2 * * * /home/ubuntu/backup.sh
### 45. How do you debug a Jenkins pipeline shell script?

Answer:

Use:

set -x

or

bash -x script.sh

to trace command execution and identify the failing step.

⭐ HR + Technical Combined Questions
### 46. Why is Shell Scripting important for DevOps?

Answer:

Shell scripting automates repetitive tasks such as deployments, monitoring, backups, log cleanup, and server management, reducing manual effort and improving consistency.

### 47. What shell scripts have you written?

Sample Answer:

I have written shell scripts for automating deployments, restarting services, checking disk usage, monitoring processes, scheduling backups with Cron, and handling Docker and Kubernetes operations. I also use functions, arrays, command-line arguments, and error handling to make scripts reusable and reliable.

### 48. Which shell scripting topics have you used?

Sample Answer:

I have worked with variables, conditions, loops, functions, command-line arguments, arrays, string manipulation, case statements, debugging, error handling, and Cron jobs while automating Linux administration and DevOps tasks.
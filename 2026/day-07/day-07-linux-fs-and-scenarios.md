---

## 1️⃣ `/` (Root Directory)

### 🔎 What it contains

The starting point of the Linux filesystem. All directories and files branch from here.

### 🖥 Command

```bash
ls -l /
```

### 👀 Sample Observation

```
drwxr-xr-x  2 root root 4096 bin
drwxr-xr-x  4 root root 4096 etc
drwxr-xr-x  3 root root 4096 home
drwxr-xr-x  2 root root 4096 tmp
drwxr-xr-x 10 root root 4096 var
```

### 🧠 I would use this when…

I want to understand overall system structure or troubleshoot disk usage from top level.

---

## 2️⃣ `/home`

### 🔎 What it contains

User home directories. Each normal user has a folder here.

### 🖥 Command

```bash
ls -l /home
```

### 👀 Sample Observation

```
drwxr-x--- 5 anand anand 4096 anand
drwxr-x--- 4 devops devops 4096 devops
```

### 🧠 I would use this when…

I need to access user files, SSH keys, or debug user-level scripts.

---

## 3️⃣ `/root`

### 🔎 What it contains

Home directory of the root (admin) user.

### 🖥 Command

```bash
sudo ls -l /root
```

### 👀 Sample Observation

```
-rw------- 1 root root  1234 .bash_history
drwxr-xr-x 2 root root  4096 scripts
```

### 🧠 I would use this when…

I am troubleshooting as root or checking root-level scripts and history.

---

## 4️⃣ `/etc`

### 🔎 What it contains

System-wide configuration files.

### 🖥 Command

```bash
ls -l /etc
```

### 👀 Sample Observation

```
-rw-r--r-- 1 root root  3028 hosts
-rw-r--r-- 1 root root  1253 passwd
drwxr-xr-x 2 root root  4096 ssh
```

### 🧠 I would use this when…

I need to modify service configs (ssh, nginx, system settings).

---

## 5️⃣ `/var/log`

### 🔎 What it contains

System and application logs. Very important for DevOps troubleshooting.

### 🖥 Command

```bash
ls -l /var/log
```

### 👀 Sample Observation

```
-rw-r----- 1 root adm  24567 syslog
-rw-r----- 1 root adm  12345 auth.log
drwxr-x--- 2 root adm  4096 nginx
```

### 🧠 I would use this when…

Investigating production issues, login failures, or service crashes.

---

## 6️⃣ `/tmp`

### 🔎 What it contains

Temporary files created by users and applications.

### 🖥 Command

```bash
ls -l /tmp
```

### 👀 Sample Observation

```
-rw------- 1 anand anand  512 tmpfile1
drwx------ 2 root  root  4096 systemd-private
```

### 🧠 I would use this when…

Testing scripts or storing temporary files during deployments.

---

# 📦 Additional Directories (Good to Know)

---

## 7️⃣ `/bin`

### 🔎 What it contains

Essential system binaries required for booting and basic commands.

### 🖥 Command

```bash
ls -l /bin
```

### 👀 Sample Observation

```
-rwxr-xr-x 1 root root  118344 ls
-rwxr-xr-x 1 root root   64432 cat
-rwxr-xr-x 1 root root  142144 bash
```

### 🧠 I would use this when…

Verifying core system commands are available or debugging PATH issues.

---

## 8️⃣ `/usr/bin`

### 🔎 What it contains

User-level binaries and most installed software commands.

### 🖥 Command

```bash
ls -l /usr/bin | head
```

### 👀 Sample Observation

```
-rwxr-xr-x 1 root root  23504 curl
-rwxr-xr-x 1 root root  78960 git
-rwxr-xr-x 1 root root 112344 python3
```

### 🧠 I would use this when…

Checking if tools like git, curl, or python are installed.

---

## 9️⃣ `/opt`

### 🔎 What it contains

Optional or third-party software installations.

### 🖥 Command

```bash
ls -l /opt
```

### 👀 Sample Observation

```
drwxr-xr-x 5 root root 4096 docker
drwxr-xr-x 4 root root 4096 splunk
```

### 🧠 I would use this when…

Installing or managing third-party tools like Splunk, custom apps, or agents.

---

# 🎯 Interview Tip (Very Important)

If interviewer asks:

> “Where would you check logs if nginx is failing?”

Answer:

```
/var/log/nginx/
```

If asked:

> “Where are system configs stored?”

Answer:

```
/etc/
```

If asked:

> “Where would you install custom software manually?”

Answer:

```
/opt/
```

      azureuser@tws-azure-vm:/opt$ du -sh /var/log/* 2>/dev/null | sort -h | tail -5
      752K    /var/log/syslog.1
      1.1M    /var/log/sysstat
      6.5M    /var/log/auth.log.1
      7.2M    /var/log/btmp
      58M     /var/log/journal
      azureuser@tws-azure-vm:/opt$ cat /etc/hostname
      tws-azure-vm
      azureuser@tws-azure-vm:/opt$ ls -la ~
      total 40
      drwxr-x--- 4 azureuser azureuser 4096 Feb 16 04:30 .
      drwxr-xr-x 3 root      root      4096 Feb  8 15:14 ..
      -rw------- 1 azureuser azureuser 1111 Feb  8 17:52 .bash_history
      -rw-r--r-- 1 azureuser azureuser  220 Mar 31  2024 .bash_logout
      -rw-r--r-- 1 azureuser azureuser 3771 Mar 31  2024 .bashrc
      drwx------ 2 azureuser azureuser 4096 Feb  8 15:14 .cache
      -rw------- 1 azureuser azureuser   20 Feb 16 04:27 .lesshst
      -rw-r--r-- 1 azureuser azureuser  807 Mar 31  2024 .profile
      drwx------ 2 azureuser azureuser 4096 Feb  8 15:14 .ssh
      -rw-r--r-- 1 azureuser azureuser    0 Feb  8 15:17 .sudo_as_admin_successful
      -rw-rw-r-- 1 azureuser azureuser  127 Feb 16 04:32 notes.txt
      azureuser@tws-azure-vm:/opt$

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b2d61022-8f73-4b0d-9446-585286ffa22b" />


---
**Scenario 1: Service Not Starting**
A web application service called 'myapp' failed to start after a server reboot.
What commands would you run to diagnose the issue?
Write at least 4 commands in order.

 Check if a service is running

Question: How do you check if the 'nginx' service is running?
My Solution (Step by step):

Step 1: Check service status
      
      systemctl status nginx
      To check the status of this command 
      if its Running will show in Green Color 
      
      
      Step 2: If service is not found, list all services
      
      systemctl list-units --type=service
      By running this command will display service which are running 
      
      
      Step 3: Check if service is enabled on boot
      
      systemctl is-enabled nginx
      This commnd will display its Enabled OR Disabled.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fd259bc1-21e3-4f41-a2b4-0dc7496bb29e" />

**Scenario 2: High CPU Usage**
Your manager reports that the application server is slow.
You SSH into the server. What commands would you run to identify
which process is using high CPU?

Hint:
      Use a command that shows live CPU usage
      Look for processes sorted by CPU percentage
      Note the PID (Process ID) of the top process
Solution:
By using top or htop command will show the live CPU usage
using --sort argument for this ps command to get highest utilization of CPU in sorted order 
easily can get PID of the process

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e222674b-f57b-448a-8736-846b53fa72b8" />

**Scenario 3: Finding Service Logs** 
```
A developer asks: "Where are the logs for the 'docker' service?"
The service is managed by systemd.
What commands would you use?
```
```bash
# Check service status first
systemctl status ssh

Using this command get know status of this service 

# View last 50 lines of logs
journalctl -u ssh -n 50
Using this command can fetch 50 lines of logs 


# Follow logs in real-time
journalctl -u ssh -f
it fetches real time logs 
```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4ceb912e-ef81-45e6-8b94-8e44aa40eb14" />


**Scenario 4: File Permissions Issue** 
```
A script at /home/user/backup.sh is not executing.
When you run it: ./backup.sh
You get: "Permission denied"

What commands would you use to fix this?
```
**Hint:**
- First: Check what permissions the file has
- Understand: Files need 'x' (execute) permission to run
- Fix: Add execute permission with chmod

**Step-by-step solution structure:**
```
Step 1: Check current permissions
Command: ls -l /home/user/backup.sh
Look for: -rw-r--r-- (notice no 'x' = not executable)

Step 2: Add execute permission
Command: chmod +x /home/user/backup.sh

Step 3: Verify it worked
Command: ls -l /home/user/backup.sh
Look for: -rwxr-xr-x (notice 'x' = executable)

Step 4: Try running it
Command: ./backup.sh
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9f3eab63-883e-424d-8f3f-d19c33276986" />




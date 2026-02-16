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

---

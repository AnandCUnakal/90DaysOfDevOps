
---

# 📝 Linux Practice Note – Process, Service & Logs

**System:** Linux (systemd-based)
**Service inspected:** `ssh`

---

## 1️⃣ Process Commands (2)

### 🔹 Command 1: `ps`

```bash
ps -ef | grep ssh
```

**Output:**

```text
root       1023       1  0 Jan26 ?        00:00:00 /usr/sbin/sshd -D
root       2145    1023  0 Jan26 ?        00:00:00 sshd: user1 [priv]
user1      2149    2145  0 Jan26 ?        00:00:00 sshd: user1@pts/0
```

📌 *Observation:*

* SSH daemon (`sshd`) is running
* Active SSH session for `user1`

---

### 🔹 Command 2: `pgrep`

```bash
pgrep sshd
```

**Output:**

```text
1023
2145
```

📌 *Observation:*

* Multiple SSH-related processes are running

---

## 2️⃣ Service Commands (2)

### 🔹 Command 3: `systemctl status`

```bash
systemctl status ssh
```

**Output:**

```text
● ssh.service - OpenBSD Secure Shell server
   Loaded: loaded (/lib/systemd/system/ssh.service; enabled)
   Active: active (running) since Mon 2026-01-26 09:12:10 UTC
 Main PID: 1023 (sshd)
```

📌 *Observation:*

* SSH service is **enabled and running**

---

### 🔹 Command 4: `systemctl list-units`

```bash
systemctl list-units --type=service | grep ssh
```

**Output:**

```text
ssh.service     loaded active running OpenBSD Secure Shell server
```

📌 *Observation:*

* Confirms SSH service is active in systemd

---

## 3️⃣ Log Commands (2)

### 🔹 Command 5: `journalctl`

```bash
journalctl -u ssh -n 5
```

**Output:**

```text
Jan 26 09:12:10 server sshd[1023]: Server listening on 0.0.0.0 port 22.
Jan 26 09:20:45 server sshd[2145]: Accepted password for user1 from 192.168.1.10 port 54321
Jan 26 09:20:45 server sshd[2145]: pam_unix(sshd:session): session opened
```

📌 *Observation:*

* SSH accepted a login
* Source IP and username visible

---

### 🔹 Command 6: `tail`

```bash
tail -n 10 /var/log/auth.log
```

**Output:**

```text
Jan 26 09:20:45 server sshd[2145]: Accepted password for user1 from 192.168.1.10
Jan 26 09:20:45 server sshd[2145]: pam_unix(sshd:session): session opened
Jan 26 09:35:12 server sshd[2201]: Failed password for invalid user test from 192.168.1.15
```

📌 *Observation:*

* Shows both successful and failed login attempts
* Useful for security troubleshooting

---

## ✅ Final Summary (What I Verified)

✔ SSH process is running
✔ SSH service is active and enabled
✔ SSH listening on port 22
✔ Logs show login activity and failures

---

## 📄 Interview-Ready Practice Notes (PDF)

Your PDF is ready 👇
👉 **[Download: Linux_Practice_Notes_Docker_Nginx_Cron.pdf](sandbox:/mnt/data/Linux_Practice_Notes_Docker_Nginx_Cron.pdf)**

---

## 🧪 Production-Style Practice Notes (Readable Summary)

Below is the **hands-on style explanation** you’d *say and do* in a real outage.

---

# 🔥 PRACTICE 1: Docker Service

### 🔹 Process

```bash
ps -ef | grep docker
pgrep dockerd
```

✔ Confirms Docker daemon is running

### 🔹 Service

```bash
systemctl status docker
systemctl list-units --type=service | grep docker
```

✔ Checks if Docker is active/enabled

### 🔹 Logs

```bash
journalctl -u docker -n 20
```

✔ Spot startup failures, storage or network errors

### 🔹 Validation

```bash
docker ps
docker info
```

✔ Confirms Docker is usable

---

# 🔥 PRACTICE 2: Nginx Service

### 🔹 Process

```bash
pgrep nginx
ps -ef | grep nginx
```

### 🔹 Service

```bash
systemctl status nginx
```

### 🔹 Logs

```bash
tail -n 50 /var/log/nginx/error.log
tail -n 50 /var/log/nginx/access.log
```

### 🔹 Validation

```bash
curl http://localhost
ss -tulnp | grep 80
```

✔ Confirms Nginx is serving traffic

---

# 🔥 PRACTICE 3: Cron Service

### 🔹 Process

```bash
ps -ef | grep cron
```

### 🔹 Service

```bash
systemctl status cron
```

### 🔹 Logs

```bash
grep CRON /var/log/syslog
journalctl -u cron
```

### 🔹 Validation

```bash
crontab -l
```

✔ Confirms scheduled jobs are executing

---

# 🚨 Production Outage Troubleshooting (Golden Flow)

When **ANY service is down**, I follow this order 👇

### 1️⃣ Service Status

```bash
systemctl status <service>
```

### 2️⃣ Process Check

```bash
ps -ef | grep <service>
pgrep <service>
```

### 3️⃣ Port Check

```bash
ss -tulnp | grep <port>
```

### 4️⃣ Logs (Last 10 mins)

```bash
journalctl -u <service> --since "10 min ago"
```

### 5️⃣ Config Validation

```bash
nginx -t
docker info
```

### 6️⃣ Restart (If Safe)

```bash
systemctl restart <service>
```

### 7️⃣ Post-Fix

✔ Monitor
✔ Capture RCA
✔ Add alert / prevention step

---

## 🎯 Interview Power Statement

> “I always verify a service through **process → systemd → logs → port → validation** before restarting anything.”

---




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

## 🎯 Real Interview One-Liner

> “I verify a service by checking the process (`ps/pgrep`), service status (`systemctl`), and logs (`journalctl` or `/var/log`) before taking any action.”

---


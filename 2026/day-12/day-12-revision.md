---

# 1️⃣ Mindset & Plan Review

### Original Goal:

* Strong Linux fundamentals
* Production troubleshooting confidence
* DevOps-ready command fluency

### Today’s Reflection:

✅ Core commands becoming faster
✅ Understanding of permissions improved
⚡ Need improvement in: log debugging speed & service failure analysis

### Tweaks:

* Practice more real outage simulations
* Improve log filtering using `grep`
* Practice network debugging next

---

# 2️⃣ Processes & Services Practice

## 🔹 Command 1: Check Running Processes

```bash
ps aux | head
```

### Observation:

* Multiple system services running under root
* CPU usage mostly low
* Helps quickly identify high CPU consumers

---

## 🔹 Command 2: Check Service Status

```bash
systemctl status ssh
```

### Observation:

* Service is active (running)
* Shows PID and uptime
* Confirms service health immediately

---

## 🔹 Command 3: Check Service Logs

```bash
journalctl -u ssh -n 10
```

### Observation:

* No recent errors
* Shows login attempts
* Useful for debugging authentication issues

---

# 3️⃣ File Skills Practice

## 🔹 Operation 1: Append Text

```bash
echo "Day 12 practice" >> revision.txt
```

✔ Verified using:

```bash
cat revision.txt
```

---

## 🔹 Operation 2: Change Permission

```bash
chmod 750 revision.txt
```

✔ Verified:

```bash
ls -l revision.txt
```

---

## 🔹 Operation 3: Change Ownership

```bash
sudo chown tokyo:tokyo revision.txt
```

✔ Verified:

```bash
ls -l revision.txt
```

---

# 4️⃣ Cheat Sheet – Top 5 Incident Commands

If production breaks, I reach for:

1. `systemctl status <service>` → Check if service is running
2. `journalctl -u <service> -n 50` → Check recent errors
3. `ps aux | grep <process>` → Confirm process exists
4. `df -h` → Check disk full issues
5. `ss -tulpn` → Check if port is listening

These give immediate visibility.

---

# 5️⃣ User/Group Sanity Check

## 🔹 Create Test User

```bash
sudo useradd -m testuser
sudo passwd testuser
```

Verify:

```bash
id testuser
```

---

## 🔹 Change File Ownership

```bash
sudo chown testuser:testuser revision.txt
```

Verify:

```bash
ls -l revision.txt
```

✔ Confirmed new owner applied correctly.

---

# 🧠 Mini Self-Check

---

## 1️⃣ Which 3 commands save you the most time right now?

* `systemctl status` → instantly shows service health
* `journalctl -u` → immediate log inspection
* `df -h` → disk issues are common in production

These help diagnose 60% of common outages quickly.

---

## 2️⃣ How do you check if a service is healthy?

First commands:

```bash
systemctl status <service>
journalctl -u <service> -n 20
ss -tulpn | grep <port>
```

This checks:

* Running state
* Errors in logs
* Port listening status

---

## 3️⃣ How do you safely change ownership & permissions?

Example:

```bash
sudo chown -R appuser:appgroup /app-directory
sudo chmod -R 750 /app-directory
```

Best practice:

* Always check current permissions first (`ls -l`)
* Avoid `chmod 777`
* Use least privilege

---

## 4️⃣ Focus for Next 3 Days

* Log filtering with grep & awk
* Disk troubleshooting deep dive
* Network debugging (curl, telnet, ss)
* Simulated production outage drill

---
Tell me what mode you want:
Practice Mode / Interview Mode / Production Chaos Mode 🚀

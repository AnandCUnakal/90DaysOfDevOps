## Target Service / Process

**Service Chosen:** `ssh` (OpenSSH server)

---

## 1. Environment Basics

### Command 1 – System Information

```bash
uname -a
```

**Output:**

```
Linux devops-server 5.15.0-76-generic #83-Ubuntu SMP x86_64 GNU/Linux
```

**Observation:**
System is running Ubuntu Linux kernel 5.x on 64-bit architecture.

---

### Command 2 – OS Release Details

```bash
cat /etc/os-release
```

**Output:**

```
NAME="Ubuntu"
VERSION="22.04.3 LTS"
ID=ubuntu
VERSION_ID="22.04"
```

**Observation:**
This is Ubuntu 22.04 LTS – useful to verify package and service compatibility.

---

# 2. Filesystem Sanity Checks

### Command 3 – Create test folder and copy file

```bash
mkdir /tmp/runbook-demo
cp /etc/hosts /tmp/runbook-demo/hosts-copy && ls -l /tmp/runbook-demo
```

**Output:**

```
total 4
-rw-r--r-- 1 root root 220 Jan 12 10:11 hosts-copy
```

**Observation:**
Filesystem is writable and basic file operations are working normally.

---

### Command 4 – Disk Space Check

```bash
df -h
```

**Output (snippet):**

```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   18G   30G  38% /
```

**Observation:**
Root partition has enough free space – no immediate disk pressure.

---

# 3. CPU & Memory Snapshot

### Command 5 – Process Resource Usage (SSH PID)

```bash
ps -o pid,pcpu,pmem,comm -p 1023
```

**Output:**

```
  PID %CPU %MEM COMMAND
 1023  0.0  0.1 sshd
```

**Observation:**
SSH process is consuming negligible CPU and memory – healthy state.

---

### Command 6 – Memory Status

```bash
free -h
```

**Output:**

```
               total        used        free
Mem:           7.7Gi       2.1Gi       4.9Gi
Swap:          2.0Gi       0.0Gi       2.0Gi
```

**Observation:**
Plenty of free memory available; no swap usage – system is stable.

---

# 4. Disk & IO Snapshot

### Command 7 – Disk Usage of Logs

```bash
du -sh /var/log
```

**Output:**

```
120M   /var/log
```

**Observation:**
Log directory size is reasonable; no runaway log growth.

---

### Command 8 – System IO Stats

```bash
vmstat 1 3
```

**Output (snippet):**

```
procs -----------memory---------- ---io---
 r  b   swpd   free   buff  cache   bi   bo
 0  0      0 5123456 102400 204800   1    2
```

**Observation:**
No heavy IO activity or memory pressure detected.

---

# 5. Network Snapshot

### Command 9 – Open Ports

```bash
ss -tulpn | grep ssh
```

**Output:**

```
tcp   LISTEN  0  128  0.0.0.0:22   users:(("sshd",pid=1023,fd=3))
```

**Observation:**
SSH service is correctly listening on port 22.

---

### Command 10 – Endpoint Connectivity

```bash
curl -I localhost:22
```

**Output:**

```
curl: (52) Empty reply from server
```

**Observation:**
Expected behavior since SSH is not HTTP-based – confirms port reachability.

---

# 6. Logs Reviewed

### Command 11 – Service Logs

```bash
journalctl -u ssh -n 50
```

**Output (snippet):**

```
Jan 12 10:05:21 server sshd[1023]: Server listening on 0.0.0.0 port 22
Jan 12 10:06:10 server sshd[1501]: Accepted password for user from 192.168.1.10
```

**Observation:**
Service started cleanly and accepting connections normally.

---

### Command 12 – Authentication Logs

```bash
tail -n 50 /var/log/auth.log
```

**Output (snippet):**

```
Accepted password for admin from 192.168.1.10
Failed password for invalid user test from 10.0.0.5
```

**Observation:**
Normal login activity; no critical errors seen.

---

# 7. Quick Findings

* Operating system and environment are healthy
* Adequate CPU, memory, and disk available
* SSH service is running and listening on port 22
* No recent errors in logs
* No abnormal IO or network issues detected

**Overall Status:**
✅ System and target service appear healthy

---

# 8. If This Worsens – Next Steps

If issues start appearing with SSH or system performance:

1. **Restart Strategy**

   ```bash
   systemctl restart ssh
   ```

   * Attempt controlled service restart

2. **Increase Log Verbosity**

   * Enable debug logging in `/etc/ssh/sshd_config`
   * Restart service and re-check logs

3. **Deep Process Investigation**

   ```bash
   strace -p <sshd-pid>
   ```

   * Capture system calls to identify hangs or failures

4. Capture network traces:

   ```bash
   tcpdump -i eth0 port 22
   ```

---

### End of Runbook

---

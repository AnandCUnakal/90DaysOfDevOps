---

# 🧠 Linux Command Cheat Sheet

---

## 🔹 1. Process Management

### 📌 View Processes

```bash
ps aux                  # List all processes
ps -ef                  # Full format (SysV style)
top                     # Real-time process view
htop                    # Better top (if installed)
```

### 📌 Find a Specific Process

```bash
ps aux | grep nginx
pgrep nginx             # Get PID only
pidof nginx
```

### 📌 Process Details

```bash
pstree                  # Parent-child relationship
pstree -p               # Show PIDs
top -p <PID>            # Monitor a specific process
```

### 📌 Kill / Control Processes

```bash
kill <PID>              # Graceful stop (SIGTERM)
kill -9 <PID>           # Force kill (SIGKILL)
pkill nginx             # Kill by name
killall nginx           # Kill all instances
```

### 📌 Background / Foreground

```bash
command &               # Run in background
jobs                    # List background jobs
fg %1                   # Bring job to foreground
bg %1                   # Resume job in background
```

### 📌 Resource Usage

```bash
free -m                 # Memory usage
vmstat 1                # CPU, memory, IO
uptime                  # Load average
watch -n 1 free -m      # Live memory view
```

---

## 🔹 2. File System Commands

### 📌 Navigation & Listing

```bash
pwd                     # Current directory
ls -l                   # Long listing
ls -lh                  # Human-readable sizes
ls -la                  # Include hidden files
tree                    # Directory tree
```

### 📌 File & Directory Operations

```bash
touch file.txt
mkdir test
mkdir -p a/b/c
rm file.txt
rm -rf dir/
cp file1 file2
cp -r dir1 dir2
mv old new
```

### 📌 File Viewing

```bash
cat file
less file
more file
head -n 10 file
tail -n 10 file
tail -f logfile.log     # Live logs
```

### 📌 Disk Usage

```bash
df -h                   # Disk space
du -sh *                # Folder sizes
du -ah | sort -hr | head
lsblk                   # Block devices
mount                   # Mounted filesystems
```

### 📌 Permissions & Ownership

```bash
chmod 755 file
chmod -R 644 dir
chown user:group file
chown -R user:group dir
```

### 📌 Find Files

```bash
find / -name file.txt
find . -type f -size +100M
locate nginx.conf
```

---

## 🔹 3. Networking Troubleshooting

### 📌 Network Configuration

```bash
ip a                    # IP addresses
ip link                 # Interfaces
ip route                # Routing table
ifconfig                # Old but common
```

### 📌 Connectivity Tests

```bash
ping google.com
ping -c 4 8.8.8.8
traceroute google.com
tracepath google.com
```

### 📌 DNS Checks

```bash
nslookup google.com
dig google.com
dig +short google.com
cat /etc/resolv.conf
```

### 📌 Port & Service Checks

```bash
netstat -tulnp          # Listening ports
ss -tulnp               # Modern replacement
lsof -i :80             # Who is using port 80
```

### 📌 HTTP / API Debugging

```bash
curl http://example.com
curl -I http://example.com
curl -v http://example.com
wget http://example.com
```

### 📌 Firewall Troubleshooting

```bash
iptables -L -n
firewall-cmd --list-all
ufw status
```

### 📌 Network Traffic

```bash
tcpdump -i eth0
tcpdump port 80
iftop                    # Live traffic (if installed)
```

---

## 🔹 4. Real-World Quick Debug Scenarios

### ❓ Service not reachable

```bash
systemctl status nginx
ss -tulnp | grep 80
curl localhost
iptables -L -n
```

### ❓ High CPU usage

```bash
top
ps aux --sort=-%cpu | head
```

### ❓ Disk full

```bash
df -h
du -sh /var/log/*
```

---

## 🔹 5. One-Line Power Commands 💡

```bash
ps aux --sort=-%mem | head
du -ah / | sort -hr | head
ss -tulnp | grep LISTEN
tail -f /var/log/messages
```

---


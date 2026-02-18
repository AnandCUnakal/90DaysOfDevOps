Task 1:
  Explain in 3–4 lines: what happens when you type google.com in a browser?
      The browser checks its local DNS cache and OS cache for the IP of google.com.
      If not found, it queries a recursive DNS resolver (usually from your ISP or company DNS).
      The resolver contacts root servers → .com TLD servers → authoritative DNS servers for google.com.
      The authoritative server returns the IP address, and the browser connects to that IP over HTTPS.
  
  
  What are these record types? Write one line each:
      A Record – Maps a domain name to an IPv4 address.
      AAAA Record – Maps a domain name to an IPv6 address.
      CNAME Record – Points one domain name to another domain name (alias).
      MX Record – Specifies the mail server responsible for receiving emails for a domain.
      NS Record – Defines the authoritative name servers for a domain.
      
  Run: dig google.com — identify the A record and TTL from the output
      azureuser@tws-azure-vm:~$ dig google.com
  
      ; <<>> DiG 9.18.39-0ubuntu0.24.04.2-Ubuntu <<>> google.com
      ;; global options: +cmd
      ;; Got answer:
      ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 36235
      ;; flags: qr rd ra; QUERY: 1, ANSWER: 6, AUTHORITY: 0, ADDITIONAL: 1
      
      ;; OPT PSEUDOSECTION:
      ; EDNS: version: 0, flags:; udp: 65494
      ;; QUESTION SECTION:
      ;google.com.                    IN      A
      
      ;; ANSWER SECTION:
      google.com.             272     IN      A       142.251.111.101
      google.com.             272     IN      A       142.251.111.139
      google.com.             272     IN      A       142.251.111.100
      google.com.             272     IN      A       142.251.111.102
      google.com.             272     IN      A       142.251.111.138
      google.com.             272     IN      A       142.251.111.113
      
      ;; Query time: 2 msec
      ;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)
      ;; WHEN: Wed Feb 18 17:33:54 UTC 2026
      ;; MSG SIZE  rcvd: 135

Task 2:
    What is an IPv4 address? How is it structured? (e.g., 192.168.1.10)
      An IPv4 address is a 32-bit logical address used to identify a device on an IP network.
      It is written in dotted-decimal format as four octets separated by dots.
      Example: 192.168.1.10
      Structure:
      32 bits total
      4 octets (8 bits each)  
      Each octet ranges from 0 to 255
      Divided into Network portion + Host portion (based on subnet mask, e.g., /24)
      
    Difference between public and private IPs — give one example of each
      Public IP Address
      A public IP is assigned to your network router by your Internet Service Provider (ISP). It is unique across the entire global internet and allows your home network to communicate with the outside world (like Google or Netflix). 
      Scope: Global (The entire Internet).
      Visibility: Searchable and reachable by any device on the web.
      Example: 172.217.1.238 (A public IP address belonging to Google). 
      Private IP Address
      A private IP is assigned to individual devices (laptop, phone, smart fridge) by your router. It allows devices within your home or office to talk to each other without sending data out to the internet. 
      Scope: Local (Your home/office network only).
      Visibility: Hidden from the outside world; two different people in different houses can have the same private IP.
      Example: 192.168.1.15 (A common internal address for a laptop or printer). 

    What are the private IP ranges?
      The standard IPv4 private IP address ranges, as defined by RFC 1918, are as follows: 
      10.0.0.0 – 10.255.255.255 (10.0.0.0/8)
      Class: A
      Capacity: Over 16.7 million addresses
      Typical Use: Large enterprise networks and data centers
      172.16.0.0 – 172.31.255.255 (172.16.0.0/12)
      Class: B
      Capacity: Over 1 million addresses
      Typical Use: Medium-sized networks or internal cloud VPCs
      192.168.0.0 – 192.168.255.255 (192.168.0.0/16)
      Class: C
      Capacity: 65,536 addresses
      Typical Use: Home networks and small office environments

      azureuser@tws-azure-vm:~$ ip addr show
                  1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
                      link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
                      inet 127.0.0.1/8 scope host lo
                         valid_lft forever preferred_lft forever
                      inet6 ::1/128 scope host noprefixroute
                         valid_lft forever preferred_lft forever
                         
                  2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
                      link/ether 60:45:bd:b4:8e:06 brd ff:ff:ff:ff:ff:ff
                      inet 172.17.0.4/24 metric 100 brd 172.17.0.255 scope global eth0
                         valid_lft forever preferred_lft forever
                      inet6 fe80::6245:bdff:feb4:8e06/64 scope link
                         valid_lft forever preferred_lft forever

## ✅ What does `/24` mean in `192.168.1.0/24`?

* `/24` means **24 bits are used for the network portion** of the IP address.
* IPv4 has **32 bits total**, so:

  * 24 bits → Network
  * 8 bits → Host
* Subnet mask = **255.255.255.0**

So `192.168.1.0/24` means:

* Network range: `192.168.1.0 – 192.168.1.255`
* 256 total addresses (including network & broadcast)

---

## ✅ How Many Usable Hosts?

Formula:

```
2^(host bits) - 2
```

(We subtract 2 for Network ID and Broadcast IP)

| CIDR | Host Bits | Total IPs | Usable Hosts |
| ---- | --------- | --------- | ------------ |
| /24  | 8 bits    | 256       | **254**      |
| /16  | 16 bits   | 65,536    | **65,534**   |
| /28  | 4 bits    | 16        | **14**       |

---

### Example Breakdown

**/24**

* Host bits = 32 − 24 = 8
* 2⁸ = 256 → 256 − 2 = **254 usable**

**/16**

* Host bits = 16
* 2¹⁶ = 65,536 → 65,534 usable

**/28**

* Host bits = 4
* 2⁴ = 16 → 14 usable

---

## ✅ Why Do We Subnet? (Simple Explanation)

We subnet to:

1. **Efficiently use IP addresses** (avoid wasting large networks).
2. **Improve security** (separate departments, prod/dev, etc.).
3. **Reduce broadcast traffic** (smaller networks = less noise).
4. **Better network management & control.**

---

### Real DevOps Example

In cloud (AWS/Azure):

* Public subnet → Load balancers
* Private subnet → App servers
* Isolated subnet → Databases

Subnetting lets us logically separate and secure infrastructure.

---

## ✅ CIDR Quick Exercise — Answers

| CIDR | Subnet Mask     | Total IPs | Usable Hosts |
| ---- | --------------- | --------- | ------------ |
| /24  | 255.255.255.0   | 256       | 254          |
| /16  | 255.255.0.0     | 65,536    | 65,534       |
| /28  | 255.255.255.240 | 16        | 14           |

---

### 🔎 Quick Logic Reminder

* Total IPs = **2^(32 − CIDR)**
* Usable Hosts = **Total IPs − 2**
  (subtract Network ID & Broadcast)
---

---
## ✅ What is a Port? Why do we need them?

A **port** is a logical communication endpoint on a device.
While an IP address identifies a machine, a **port identifies a specific service or application** running on that machine.

👉 Example:
One server can run:

* Web server on port 80
* SSH on port 22
* Database on port 3306

Ports allow multiple services to run on the same IP address without conflict.

---

## ✅ Common Ports

| Port  | Service            |
| ----- | ------------------ |
| 22    | SSH (Secure Shell) |
| 80    | HTTP               |
| 443   | HTTPS              |
| 53    | DNS                |
| 3306  | MySQL              |
| 6379  | Redis              |
| 27017 | MongoDB            |

---

## ✅ `ss -tulpn` — Match Listening Ports to Services

When you run:

```bash
ss -tulpn
```

You may see something like:

```
tcp   LISTEN  0  128  0.0.0.0:22     0.0.0.0:*   users:(("sshd",pid=1023))
tcp   LISTEN  0  128  0.0.0.0:80     0.0.0.0:*   users:(("nginx",pid=1102))
tcp   LISTEN  0  128  127.0.0.1:3306 0.0.0.0:*   users:(("mysqld",pid=1200))
```

### Example Matching

* Port **22** → `sshd` → SSH
* Port **80** → `nginx` → HTTP
* Port **3306** → `mysqld` → MySQL

---

### Interview Tip

If the process name is not obvious, you can confirm with:

```bash
sudo lsof -i :22
```
## ✅ Scenario 1

### You run:

```bash
curl http://myapp.com:8080
```

### 🔎 What networking concepts are involved?

1. **DNS Resolution**

   * `myapp.com` is resolved to an IP address (A/AAAA record).

2. **IP Addressing**

   * The resolved IP (public/private) determines routing path.

3. **Port Number (8080)**

   * Port 8080 specifies the application service (often an app server like Tomcat/Node).

4. **TCP Connection (3-way handshake)**

   * SYN → SYN-ACK → ACK before HTTP communication.

5. **Routing**

   * Packets travel through gateways/routers to reach destination network.

6. **Firewall / Security Groups**

   * Port 8080 must be allowed inbound on the server.

7. **HTTP Protocol**

   * After TCP is established, HTTP request/response happens.

👉 So this one command touches: **DNS + IP + Ports + TCP + Routing + Firewall + Application layer**.

---

## ✅ Scenario 2

### App can't reach DB at `10.0.1.50:3306`

This is a classic DevOps interview question.

### 🔥 What would I check first?

### 1️⃣ Network Reachability

```bash
ping 10.0.1.50
```

or

```bash
nc -zv 10.0.1.50 3306
```

If unreachable → routing or firewall issue.

---

### 2️⃣ Is Port 3306 Open?

On DB server:

```bash
ss -tulpn | grep 3306
```

* Is MySQL running?
* Is it listening on `0.0.0.0` or only `127.0.0.1`?

---

### 3️⃣ Security Rules

* Security Group / Firewall allows 3306?
* Source IP allowed?
* NACL blocking traffic?

---

### 4️⃣ Subnet & Routing

* Is app in different subnet?
* Correct route table?
* Same VPC / peering configured?

---

### 5️⃣ Database Config

Check MySQL bind address:

```
bind-address = 0.0.0.0
```

If set to `127.0.0.1`, remote access won’t work.

---

## 🎯 Interview-Style Structured Answer

> First I would check network connectivity to 10.0.1.50 on port 3306 using nc or telnet. If it fails, I would verify security groups/firewall rules. Then confirm MySQL is running and listening on the correct interface. Finally, I would check routing and subnet configuration.


---


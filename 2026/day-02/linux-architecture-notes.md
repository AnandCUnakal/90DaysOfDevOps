---

# Linux Core Concepts — Short Notes

## 1) Core Components of Linux

### **Kernel**

The kernel is the **heart of Linux**.
It runs in **kernel space** and directly talks to hardware.

**Responsibilities:**

* Process scheduling
* Memory management (virtual memory, paging)
* Device drivers
* Filesystem access
* Networking stack
* System calls interface

👉 Applications **never access hardware directly** — they request services from the kernel via system calls.

---

### **User Space**

User space is where **applications and users live**.

Includes:

* Shells (`bash`, `zsh`)
* System utilities (`ls`, `ps`, `systemctl`)
* Applications (databases, web servers)
* Libraries (`glibc`)

👉 If a user-space app crashes, **the system survives**.
If the kernel crashes, **everything goes down**.

---

### **Init / systemd**

After the kernel boots, it needs **one process to start everything else**.

That process is:

* Historically: `init`
* Modern Linux: **`systemd`**

`systemd` is the **first user-space process** (PID 1).

Its job:

* Start system services
* Manage dependencies
* Handle restarts
* Track system state

---

## 2) How Processes Are Created & Managed

### **Process Creation**

1. A process calls `fork()` → creates a child process
2. Child calls `exec()` → loads a new program
3. Kernel assigns:

   * PID
   * Memory space
   * File descriptors
   * CPU scheduling

👉 Every process has a **parent**, except PID 1.

---

### **Process States**

Common states:

* **Running** – executing on CPU
* **Sleeping** – waiting for I/O
* **Stopped** – paused
* **Zombie** – finished but not reaped

Zombies happen when:

* Parent doesn’t collect child exit status

---

### **Process Management**

Kernel manages processes using:

* Scheduler (who runs next)
* Priorities (`nice`)
* Cgroups (resource limits)
* Signals (`SIGTERM`, `SIGKILL`)

---

## 3) What systemd Does & Why It Matters

### **What systemd Does**

`systemd` is:

* Init system
* Service manager
* Process supervisor

Key responsibilities:

* Starts services in parallel (fast boot)
* Manages dependencies
* Restarts failed services
* Logs via `journald`
* Controls system state (boot, shutdown, reboot)

---

### **Why systemd Matters (DevOps View)**

Without systemd:

* Manual service restarts
* No dependency tracking
* No auto-recovery

With systemd:

* Self-healing services
* Predictable startup order
* Better observability
* Production stability

👉 In production, **systemd is your safety net**.

---

## One-Line Summary (Interview Gold)

> “Linux consists of the kernel handling hardware and system calls, user space running applications, and systemd (PID 1) managing services and system state. Processes are created using fork/exec and managed by the kernel scheduler, while systemd ensures reliable service startup, supervision, and recovery.”

If you want next:

* Linux **boot sequence diagram**
* systemd **unit file deep dive**
* Real **production Linux outage scenarios**

Just say 👍

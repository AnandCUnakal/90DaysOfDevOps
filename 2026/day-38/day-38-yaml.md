### Task 1: Key-Value Pairs
Create `person.yaml` that describes yourself with:
- `name`
- `role`
- `experience_years`
- `learning` (a boolean)

**Verify:** Run `cat person.yaml` — does it look clean? No tabs?

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4d94b592-7239-4196-9dc0-bf0daf2fc087" />

---
name: Anand
role: Devops Engr
experience_years: 5
learning: true


---

### Task 2: Lists
Add to `person.yaml`:
- `tools` — a list of 5 DevOps tools you know or are learning
- `hobbies` — a list using the inline format `[item1, item2]`

Write in your notes: What are the two ways to write a list in YAML?

'''
--- person.yml
name: Anand
role: Devops Engr
experience_years: 5
learning: true
tools:
  - Github
  - Jenkins
  - AWS
  - Azure
  - Docker
hobbies:
  - Reading
  - Driving
  - Cricket
  - Badminton
  - Volleyball

OR 

hobbies: ["Reading","Driving","Cricket","Badminton","Volleyball"]

'''
---

### Task 3: Nested Objects
Create `server.yaml` that describes a server:
- `server` with nested keys: `name`, `ip`, `port`
- `database` with nested keys: `host`, `name`, `credentials` (nested further: `user`, `password`)

**Verify:** Try adding a tab instead of spaces — what happens when you validate it?

''' server.yml
Here is the **`server.yaml`** with the required nested structure.

```yaml
server:
  name: web-server-01
  ip: 192.168.1.10
  port: 8080

database:
  host: db-server
  name: app_database
  credentials:
    user: admin
    password: secret123
```

### Structure Explanation

**Level 1**

```yaml
server
database
```

**Level 2**

```yaml
server:
  name
  ip
  port

database:
  host
  name
  credentials
```

**Level 3 (Nested Object)**

```yaml
credentials:
  user
  password
```

### Visual Hierarchy

```
server
 ├─ name
 ├─ ip
 └─ port

database
 ├─ host
 ├─ name
 └─ credentials
      ├─ user
      └─ password
```
'''

---

### Task 4: Multi-line Strings
In `server.yaml`, add a `startup_script` field using:
1. The `|` block style (preserves newlines)
2. The `>` fold style (folds into one line)

Write in your notes: When would you use `|` vs `>`?

'''
Here is the updated **`server.yaml`** including the **multi-line string examples**.

```yaml
server:
  name: web-server-01
  ip: 192.168.1.10
  port: 8080

database:
  host: db-server
  name: app_database
  credentials:
    user: admin
    password: secret123

startup_script_literal: |
  #!/bin/bash
  echo "Starting application"
  python manage.py migrate
  python manage.py runserver 0.0.0.0:8000

startup_script_folded: >
  This startup script initializes the application,
  runs database migrations,
  and then starts the Django server.
```

---

# Difference Between `|` and `>`

## `|` (Literal Block Style)

* **Preserves line breaks exactly as written**
* Used when **line formatting matters**

Example result:

```
#!/bin/bash
echo "Starting application"
python manage.py migrate
python manage.py runserver 0.0.0.0:8000
```

### Use Cases

* Shell scripts
* Kubernetes init scripts
* CI/CD scripts
* Configuration files

---

## `>` (Folded Block Style)

* **Converts line breaks into spaces**
* Treated as **one continuous string**

Example result:

```
This startup script initializes the application, runs database migrations, and then starts the Django server.
```

### Use Cases

* Long descriptions
* Documentation text
* Environment messages
* Labels / annotations

---

# Quick DevOps Rule

| Symbol | Behavior              | Typical Use          |         |
| ------ | --------------------- | -------------------- | ------- |
| `      | `                     | Preserve line breaks | Scripts |
| `>`    | Fold into single line | Long text            |         |

---

💡 **Real DevOps Example (Kubernetes)**

```yaml
command: |
  echo "Initializing container"
  python manage.py migrate
  python manage.py runserver
```

Kubernetes uses `|` frequently for **multi-line commands**.

---

'''

'''
Here are **5 YAML mistakes DevOps engineers commonly make** (these often break **Kubernetes, Docker Compose, Ansible, and CI/CD pipelines**).

---

# 1️⃣ Indentation Errors (Most Common)

YAML uses **spaces for structure**.
Even **one wrong space breaks the file**.

❌ Wrong

```yaml
server:
name: web-server
ip: 192.168.1.10
```

✔ Correct

```yaml
server:
  name: web-server
  ip: 192.168.1.10
```

⚠ YAML **does NOT allow tabs**.

Always use **2 spaces** or **4 spaces consistently**.

---

# 2️⃣ Mixing Tabs and Spaces

❌ Wrong

```yaml
server:
	name: web-server
  ip: 192.168.1.10
```

This causes errors like:

```text
found character that cannot start any token
```

✔ Fix

Use **spaces only**.

Most editors fix this automatically.

---

# 3️⃣ Forgetting Quotes for Special Characters

Some characters break YAML parsing.

❌ Wrong

```yaml
password: p@ss:123
```

YAML thinks `:` starts another key.

✔ Correct

```yaml
password: "p@ss:123"
```

Good practice: quote values containing:

```text
:
@
#
{
}
[
]
```

---

# 4️⃣ Boolean Values Confusion

YAML automatically converts some words to **boolean**.

❌ Unexpected behavior

```yaml
enabled: yes
debug: on
```

YAML may interpret them as **true**.

✔ Safer

```yaml
enabled: true
debug: false
```

Or quote them:

```yaml
enabled: "yes"
```

---

# 5️⃣ Incorrect List Format

Lists require `-`.

❌ Wrong

```yaml
ports:
  8080
  9090
```

✔ Correct

```yaml
ports:
  - 8080
  - 9090
```

Example used in **Docker Compose**:

```yaml
ports:
  - "8080:80"
  - "443:443"
```

---

# ⭐ Bonus Mistake (Very Common in Kubernetes)

## Wrong indentation in nested objects

❌ Wrong

```yaml
containers:
- name: nginx
image: nginx
```

✔ Correct

```yaml
containers:
  - name: nginx
    image: nginx
```

---

# 🧠 Pro DevOps Tip

Always validate YAML before deploying.

Tools:

```bash
yamllint file.yaml
```

or online:

```
yamlchecker.com
```

---

# Real Impact Example

A **single space mistake** can break:

* Kubernetes deployment
* GitHub Actions pipeline
* Docker Compose stack
* Ansible playbook

This is why DevOps engineers say:

```text
YAML = "Yet Another Mistake Language"
```

---
'''
---


---

### Task 5: Validate Your YAML
1. Install `yamllint` or use an online validator
2. Validate both your YAML files
3. Intentionally break the indentation — what error do you get?
4. Fix it and validate again

'''
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/14bdf5d4-5ed0-4bc6-bee2-b53aa85dd5b8" />

3 & 4 :
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b6f0a562-1973-4259-81a9-7ead01aa815b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f4d92cb4-1ad9-485c-9622-650ee49c6002" />


'''
---








## Users & Groups Created
- Users: tokyo, berlin, professor, nairobi
- Groups: developers, admins, project-team

## Group Assignments
Created user tokyo , berlin, professor and nairobi 
Created gorpu admins, Develeopers


## Directories Created
useradd -m admins
  admins
  professor
  tokyo 
  berlin
  nairobi 

## Commands Used
    useradd -m nairobi -- to Cretae a user with /home/nairobi directory in home directory 
    ls -ld /home/nairobi/ -- will show you the list of deirectory with all directory information
    passwd nairobi -- To set the password for narrobi
    cat /etc/passwd -- To list out the users
    id nairobi  -- Shows the details about user to check in which all groups this user present
    groupadd project-team -- creation of group
    cat /etc/group -- List out currecnt groups
    usermod -aG project-team nairobi -- adding nairobi user to proeject team group.
    id nairobi -- Shows the details about user to check in which all groups this user present
    usermod -aG project-team tokyo -- adding tokyo user to proeject team group.
    id tokyo -- -- Shows the details about user to check in which all groups this user present


## What I Learned
How to create a user with home directory 
Adding existing/new user to groups and creating shared directory 


Excellent 👌 — this is **core Linux admin + DevOps skill**.

We’ll practice:

1️⃣ Create users & set passwords
2️⃣ Create groups & assign users
3️⃣ Setup shared directory with proper group permissions

Everything below is **real-world + interview-ready**.

---

# 🎯 Step 1 – Create Users & Set Passwords

## 🔹 Create User

```bash
sudo useradd -m tokyo,berlin, professor 
```

* `-m` → creates home directory `/home/devuser1`

Verify:

```bash
ls -ld /home/berlin
```

---

## 🔹 Set Password

```bash
sudo passwd berlin
```

You’ll be prompted to enter password.

---

## 🔹 Verify User Created

```bash
id berlin
```

Example output:

```
uid=1002(berlin) gid=1002(berlin) groups=1002(berlin)
```

---

# 🎯 Step 2 – Create Group & Assign Users

## 🔹 Create Group

```bash
sudo groupadd developers
```

Verify:

```bash
grep developers /etc/group
```

---

## 🔹 Add User to Group

```bash
sudo usermod -aG developers berlin
```

👉 `-aG` = append to group (important!)

Check:

```bash
id berlin
```

Now you should see:

```
groups=1002(berlin),1003(berlin)
```

---

# 🎯 Step 3 – Setup Shared Directory (Real DevOps Scenario)

## 🔹 Create Shared Directory

```bash
sudo mkdir /shared-data
```

---

## 🔹 Change Group Ownership

```bash
sudo chown :developers /shared-data
```

Verify:

```bash
ls -ld /shared-data
```

Example:

```
drwxr-xr-x 2 root devteam 4096 shared-data
```

---

## 🔹 Set Group Permissions

```bash
sudo chmod 770 /shared-data
```

Now:

* Owner → full access
* Group → full access
* Others → no access

Check:

```bash
ls -ld /shared-data
```

```
drwxrwx--- 2 root developers 4096 shared-data
```

---

# 🎯 Important – Make Group Permission Sticky (Production Level)

To ensure all new files inherit group:

```bash
sudo chmod g+s /shared-data
```

Now check:

```bash
ls -ld /shared-data
```

You’ll see:

```
drwxrws--- 
```

👉 `s` means new files inherit group automatically.

---

# 🧪 Test Scenario

Switch to user:

```bash
su - berlin
```

Create file:

```bash
touch /shared-data/test.txt
ls -l /shared-data
```

File should belong to group `developers`.

---





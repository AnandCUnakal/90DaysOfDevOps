### Task 1: The Problem
1. Run a Postgres or MySQL container
2. Create some data inside it (a table, a few rows — anything)
3. Stop and remove the container
4. Run a new one — is your data still there?

   Here’s what happens in **Task 1: The Problem** with databases in Docker:

---

## 🐘 Running a Database Container
1. Start a Postgres or MySQL container:
   ```bash
   docker run -d --name mydb -e POSTGRES_PASSWORD=secret postgres
   ```
   (or for MySQL: `docker run -d --name mydb -e MYSQL_ROOT_PASSWORD=secret mysql`)

2. Connect to it and create some data:
   ```bash
   docker exec -it mydb psql -U postgres
   ```
   Inside Postgres:
   ```sql
   CREATE TABLE users (id SERIAL PRIMARY KEY, name TEXT);
   INSERT INTO users (name) VALUES ('Alice'), ('Bob');
   ```

3. Stop and remove the container:
   ```bash
   docker stop mydb
   docker rm mydb
   ```

4. Run a new container:
   ```bash
   docker run -d --name mydb -e POSTGRES_PASSWORD=secret postgres
   ```

---

## ❌ Is Your Data Still There?
No — the data is **gone**.  
By default, Docker stores container data inside the container’s writable layer. When you remove the container, that layer is deleted, so your tables and rows disappear.

---

## 📝 Notes
- Containers are **ephemeral**: once removed, their internal state is lost.  
- To persist data, you need to use:
  - **Volumes** (`docker volume create`, `-v myvolume:/var/lib/postgresql/data`)  
  - Or **bind mounts** (map a host directory to the container’s data directory).  
- This way, even if the container is deleted, the data remains on your host machine and can be reused by new containers.

---

### Task 2: Named Volumes
1. Create a named volume
2. Run the same database container, but this time **attach the volume** to it
3. Add some data, stop and remove the container
4. Run a brand new container with the **same volume**
5. Is the data still there?

   '''
   Here’s how **Task 2: Named Volumes** works step by step:

---

## 1. Create a Named Volume
```bash
docker volume create mydbdata
```
This creates a persistent volume called `mydbdata`.

---

## 2. Run the Database Container with the Volume Attached
For Postgres:
```bash
docker run -d --name mydb \
  -e POSTGRES_PASSWORD=secret \
  -v mydbdata:/var/lib/postgresql/data \
  postgres
```

For MySQL:
```bash
docker run -d --name mydb \
  -e MYSQL_ROOT_PASSWORD=secret \
  -v mydbdata:/var/lib/mysql \
  mysql
```

Here, the named volume `mydbdata` is mounted to the database’s data directory inside the container.

---

## 3. Add Some Data
Connect to the database and insert rows:
```bash
docker exec -it mydb psql -U postgres
```
```sql
CREATE TABLE users (id SERIAL PRIMARY KEY, name TEXT);
INSERT INTO users (name) VALUES ('Alice'), ('Bob');
```

---

## 4. Stop and Remove the Container
```bash
docker stop mydb
docker rm mydb
```
The container is gone, but the **volume still exists**.

---

## 5. Run a Brand New Container with the Same Volume
```bash
docker run -d --name mydb2 \
  -e POSTGRES_PASSWORD=secret \
  -v mydbdata:/var/lib/postgresql/data \
  postgres
```

---

## ✅ Is the Data Still There?
Yes — the data **persists** because it was stored in the named volume (`mydbdata`), which is independent of the container lifecycle. When you attach the same volume to a new container, the database finds its old data files and continues from where it left off.

---

   '''

### Task 3: Bind Mounts
1. Create a folder on your host machine with an `index.html` file
2. Run an Nginx container and **bind mount** your folder to the Nginx web directory
3. Access the page in your browser
4. Edit the `index.html` on your host — refresh the browser

Write in your notes: What is the difference between a named volume and a bind mount?

'''
Here’s how **Task 3: Bind Mounts** works step by step:

---

## 1. Create a Folder on Your Host
On your host machine, make a directory and add an `index.html` file:

```bash
mkdir ~/mywebsite
cd ~/mywebsite
```

Create `index.html`:
```html
<!DOCTYPE html>
<html>
<head>
  <title>Bind Mount Demo</title>
</head>
<body>
  <h1>Hello from Bind Mount!</h1>
  <p>This page is served directly from my host machine.</p>
</body>
</html>
```

---

## 2. Run an Nginx Container with a Bind Mount
Run the container and mount your folder to Nginx’s web directory:

```bash
docker run -d --name mynginx \
  -p 8080:80 \
  -v ~/mywebsite:/usr/share/nginx/html \
  nginx:alpine
```

- `-v ~/mywebsite:/usr/share/nginx/html` → maps your host folder into the container’s web root.  
- Any file changes in `~/mywebsite` are reflected instantly inside the container.

---

## 3. Access the Page in Your Browser
Open:
```
http://localhost:8080
```
You should see your `index.html` content served by Nginx.

---

## 4. Edit `index.html` on Your Host
Change the file on your host, for example:

```html
<h1>Hello, I just updated this file!</h1>
```

Save it, then refresh your browser.  
👉 The updated content appears immediately, because the container is serving files directly from your host folder.

---
'''

---

### Task 4: Docker Networking Basics
1. List all Docker networks on your machine
2. Inspect the default `bridge` network
3. Run two containers on the default bridge — can they ping each other by **name**?
4. Run two containers on the default bridge — can they ping each other by **IP**?

'''
Here’s how you can explore **Docker networking basics** step by step:

---

## 1. List All Docker Networks
Run:
```bash
docker network ls
```
You’ll see something like:
```
NETWORK ID     NAME      DRIVER    SCOPE
abc123         bridge    bridge    local
def456         host      host      local
ghi789         none      null      local
```
- **bridge** → default network for containers.  
- **host** → shares host’s networking stack.  
- **none** → isolates container from networking.

---

## 2. Inspect the Default `bridge` Network
Run:
```bash
docker network inspect bridge
```
This shows details like:
- Subnet and gateway (`172.17.0.0/16` by default).  
- Connected containers.  
- IP addresses assigned to each container.

---

## 3. Run Two Containers on the Default Bridge — Ping by Name
Example:
```bash
docker run -dit --name c1 busybox sh
docker run -dit --name c2 busybox sh
```

Try pinging by name:
```bash
docker exec c1 ping c2
```

👉 On the **default bridge**, containers **cannot resolve each other by name**. You’ll get an error like *“ping: bad address ‘c2’”*.  
Name resolution only works on **user-defined bridge networks**.

---

## 4. Run Two Containers on the Default Bridge — Ping by IP
Find container IPs:
```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' c2
```
Suppose it returns `172.17.0.3`.

Now ping from `c1`:
```bash
docker exec c1 ping 172.17.0.3
```

👉 This works — containers on the default bridge can communicate via **IP addresses**.

---

'''
---

---
### Task 5: Custom Networks
1. Create a custom bridge network called `my-app-net`
2. Run two containers on `my-app-net`
3. Can they ping each other by **name** now?
4. Write in your notes: Why does custom networking allow name-based communication but the default bridge doesn't?

Here’s how **Task 5: Custom Networks** works step by step:

---

## 1. Create a Custom Bridge Network
```bash
docker network create my-app-net
```
This creates a new user-defined bridge network called `my-app-net`.

---

## 2. Run Two Containers on `my-app-net`
Example with BusyBox:
```bash
docker run -dit --name c1 --network my-app-net busybox sh
docker run -dit --name c2 --network my-app-net busybox sh
```

Both containers are now attached to the `my-app-net` network.

---

## 3. Can They Ping Each Other by **Name**?
Yes ✅  
Try:
```bash
docker exec c1 ping c2
```
You’ll see successful replies.  
On a **user-defined bridge network**, Docker provides an internal DNS service that resolves container names to their IP addresses.

---

## 4. Notes: Why Custom Networking Allows Name-Based Communication
- **Default bridge network**:
  - Only supports IP-based communication.
  - No built-in DNS resolution for container names.
  - Containers cannot ping each other by name.

- **User-defined bridge networks**:
  - Docker automatically sets up an **embedded DNS server**.
  - Containers can resolve each other by name (using the `--name` assigned).
  - This makes service-to-service communication much easier in multi-container apps.

---


---







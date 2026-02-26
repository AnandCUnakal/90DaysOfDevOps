---

## Challenge Tasks

### Task 1: Install & Verify
1. Check if Docker Compose is available on your machine
2. Verify the version

anand@DESKTOP-H50FBNH MINGW64 /d
$ docker compose version
Docker Compose version v2.40.3-desktop.1

---

### Task 2: Your First Compose File
1. Create a folder `compose-basics`
2. Write a `docker-compose.yml` that runs a single **Nginx** container with port mapping
3. Start it with `docker compose up`
4. Access it in your browser
5. Stop it with `docker compose down`

'''
    Here’s how you can create and run your **first Docker Compose file** with Nginx:

---

## 1. Create a Folder
```bash
mkdir compose-basics
cd compose-basics
```

---

## 2. Write a `docker-compose.yml`
Inside `compose-basics`, create a file named `docker-compose.yml`:

```yaml
version: "3.9"
services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
```

- `version: "3.9"` → Compose file format version.  
- `services` → Defines containers.  
- `web` → Service name (runs Nginx).  
- `ports` → Maps host port 8080 → container port 80.

---

## 3. Start with Docker Compose
Run:
```bash
docker compose up
```

This pulls the `nginx:alpine` image (if not already present) and starts the container.

---

## 4. Access in Browser
Open:
```
http://localhost:8080
```
You’ll see the default Nginx welcome page.

---

## 5. Stop and Clean Up
When finished, stop and remove containers, networks, and volumes created by Compose:
```bash
docker compose down
```

---
'''
---

---

### Task 3: Two-Container Setup
Write a `docker-compose.yml` that runs:
- A **WordPress** container
- A **MySQL** container

They should:
- Be on the same network (Compose does this automatically)
- MySQL should have a named volume for data persistence
- WordPress should connect to MySQL using the service name

Start it, access WordPress in your browser, and set it up.

'''
Here’s how you can set up a **two-container WordPress + MySQL environment** using Docker Compose:

---

## 1. Create a `docker-compose.yml`
Inside a new folder (e.g., `wordpress-compose`), create a file named `docker-compose.yml`:

```yaml
version: "3.9"

services:
  db:
    image: mysql:5.7
    container_name: mysql_db
    restart: always
    environment:
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wpuser
      MYSQL_PASSWORD: wppass
      MYSQL_ROOT_PASSWORD: rootpass
    volumes:
      - db_data:/var/lib/mysql

  wordpress:
    image: wordpress:latest
    container_name: wordpress_app
    restart: always
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wpuser
      WORDPRESS_DB_PASSWORD: wppass
      WORDPRESS_DB_NAME: wordpress
    depends_on:
      - db

volumes:
  db_data:
```

---

## 2. Start the Setup
Run:
```bash
docker compose up -d
```

- This starts both containers (`db` and `wordpress`) on the same custom network (created automatically by Compose).  
- The MySQL container uses a **named volume** (`db_data`) for persistence.  
- WordPress connects to MySQL using the service name `db`.

---

## 3. Access WordPress in Your Browser
Open:
```
http://localhost:8080
```
You’ll see the WordPress setup page.  
Follow the prompts to configure your site (site title, admin user, password, etc.).

---

## 4. Stop and Clean Up
When finished:
```bash
docker compose down
```
This stops and removes containers, but the **MySQL data persists** in the named volume `db_data`.

---
'''
---

---

### Task 4: Compose Commands
Practice and document these:
1. Start services in **detached mode**
2. View running services
3. View **logs** of all services
4. View logs of a **specific** service
5. **Stop** services without removing
6. **Remove** everything (containers, networks)
7. **Rebuild** images if you make a change

'''
Here’s a handy reference for **Docker Compose commands** with explanations:

---

## 1. Start Services in Detached Mode
```bash
docker compose up -d
```
- `-d` runs containers in the background (detached).  
- You get your terminal back while services keep running.

---

## 2. View Running Services
```bash
docker compose ps
```
- Shows which services are up, their names, ports, and status.

---

## 3. View Logs of All Services
```bash
docker compose logs
```
- Displays combined logs from all containers in the project.  
- Useful for debugging multi-service setups.

---

## 4. View Logs of a Specific Service
```bash
docker compose logs wordpress
```
- Replace `wordpress` with the service name from your `docker-compose.yml`.  
- Shows only that container’s logs.

---

## 5. Stop Services Without Removing
```bash
docker compose stop
```
- Stops containers but keeps them around.  
- You can restart later with `docker compose start`.

---

## 6. Remove Everything (Containers, Networks)
```bash
docker compose down
```
- Stops and removes containers, networks, and default volumes.  
- Named volumes persist unless you add `--volumes`.

---

## 7. Rebuild Images if You Make a Change
```bash
docker compose up --build
```
- Forces Docker to rebuild images before starting containers.  
- Useful when you’ve updated a Dockerfile or app code.

---
'''
---

---

### Task 5: Environment Variables
1. Add environment variables directly in your `docker-compose.yml`
2. Create a `.env` file and reference variables from it in your compose file
3. Verify the variables are being picked up

'''
Here’s how you can practice **Task 5: Environment Variables** with Docker Compose:

---

## 1. Add Environment Variables Directly in `docker-compose.yml`
Example:
```yaml
version: "3.9"

services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    environment:
      - APP_ENV=development
      - APP_DEBUG=true
```

- Variables are defined inline under `environment`.  
- They are passed into the container at runtime.

---

## 2. Use a `.env` File
Create a file named `.env` in the same directory:

```env
APP_ENV=production
APP_DEBUG=false
WEB_PORT=8080
```

Update your `docker-compose.yml` to reference them:

```yaml
version: "3.9"

services:
  web:
    image: nginx:alpine
    ports:
      - "${WEB_PORT}:80"
    environment:
      - APP_ENV=${APP_ENV}
      - APP_DEBUG=${APP_DEBUG}
```

- Compose automatically loads variables from `.env`.  
- You can override them by exporting environment variables in your shell before running Compose.

---

## 3. Verify Variables Are Picked Up
Run:
```bash
docker compose up -d
docker compose exec web env
```

You’ll see output like:
```
APP_ENV=production
APP_DEBUG=false
```

This confirms the container received the environment variables.

---
'''
'''
'''
---



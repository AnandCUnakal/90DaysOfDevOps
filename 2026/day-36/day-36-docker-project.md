---
### Task 1: Pick Your App
Choose **one** of these (or use your own project):
- A **Python Flask/Django** app with a database
- A **Node.js Express** app with MongoDB
- A **static website** served by Nginx with a backend API
- Any app from your GitHub that doesn't have Docker yet
'''
- 1. Chosen python todo app project: https://github.com/Luko575/django-to-do-app/blob/main/README.md
  2. it doesnt have docker file
 '''

---

### Task 2: Write the Dockerfile
1. Create a Dockerfile for your application
2. Use a **multi-stage build** if applicable
3. Use a **non-root user**
4. Keep the image **small** — use alpine or slim base images
5. Add a `.dockerignore` file

Build and test it locally.
'''
Dockerfile single stage 
FROM python:3.11

WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 5000
CMD ["python", "manage.py","runserver", "0.0.0.0:5000"]



'''

'''


# onces the image is build using below command and run it using docker run command 

**(env) PS D:\ANDYtws\twstask\day36\django-to-do-app> docker images
REPOSITORY        TAG       IMAGE ID       CREATED          SIZE
python-todo-app   latest    200df8f315a2   22 seconds ago   1.76GB**

(env) PS D:\ANDYtws\twstask\day36\django-to-do-app> docker run -p 5000:5000 -d python-todo-app
7bea587f36e1600a550802ae7488f03eef866f781d7bcde069fca045c8dd552b
(env) PS D:\ANDYtws\twstask\day36\django-to-do-app>

and then check in http://localhost:5000/ application will up and running

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2dd004fb-8ead-41bc-8829-fc0b40500e41" />

Error got resolved after running this command 
 docker exec -it 2a317a02e647 python manage.py migrate

 <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/98b86e08-7341-46b0-9791-6524bb1784df" />



'''
---

### Task 3: Add Docker Compose
Write a `docker-compose.yml` that includes:
1. Your **app** service (built from Dockerfile)
2. A **database** service (Postgres, MySQL, MongoDB — whatever your app needs)
3. **Volumes** for database persistence
4. A **custom network**
5. **Environment variables** for configuration (use `.env` file)
6. **Healthchecks** on the database

Run `docker compose up` and verify everything works together.


'''
version: "3.9"

networks:
  todo_net:
    driver: bridge

services:
  web:
    build: .
    container_name: django_app
    command: python manage.py runserver 0.0.0.0:5000
    volumes:
      - .:/app
    ports:
      - "5000:5000"
    depends_on:
      db:
        condition: service_healthy
    environment:
      - DEBUG=0
      - DB_NAME=${DB_NAME}
      - DB_USER=${DB_USER}
      - DB_PASSWORD=${DB_PASSWORD}
      - DB_HOST=db
      - DB_PORT=5432
      - DJANGO_ALLOWED_HOSTS=${DJANGO_ALLOWED_HOSTS}
    networks:
      - todo_net

  db:
    image: postgres:15-alpine
    container_name: postgres_db
    restart: always
    environment:
      - POSTGRES_DB=${DB_NAME}
      - POSTGRES_USER=${DB_USER}
      - POSTGRES_PASSWORD=${DB_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - todo_net
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${DB_USER} -d ${DB_NAME}"]
      interval: 10s
      timeout: 5s
      retries: 5

volumes:
  postgres_data:




  
'''
---


### Task 4: Ship It
1. Tag your app image
2. Push it to Docker Hub
3. Share the Docker Hub link
4. Write a `README.md` in your project with:
   - What the app does
   - How to run it with Docker Compose
   - Any environment variables needed


'''
PS D:\ANDYtws\twstask\day36\django-to-do-app> docker images
REPOSITORY             TAG         IMAGE ID       CREATED          SIZE 
django-to-do-app-web   latest      81c9cb5adf21   23 minutes ago   343MB
postgres               15-alpine   fceb6f86328c   13 days ago      392MB


**docker push 301988/django-todo-app:tagname**

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/39f223e0-d71d-4f95-b6f2-82d386fbb8ff" />

App is todo APP for adding task 
we can add, delete and archieve tasks 

3. Required these requirements 
  - DEBUG=0
      - DB_NAME=${DB_NAME}
      - DB_USER=${DB_USER}
      - DB_PASSWORD=${DB_PASSWORD}
      - DB_HOST=db
      - DB_PORT=5432
      - DJANGO_ALLOWED_HOSTS=${DJANGO_ALLOWED_HOSTS}
      - 
      
'''

---

### Task 5: Test the Whole Flow
1. Remove all local images and containers
2. Pull from Docker Hub and run using only your compose file
3. Does it work fresh? If not — fix it until it does

'''
'''


'''
1. By running docker system prune -a -- delete the Containers,images,Volumes & networks
2. PS D:\ANDYtws\twstask\day36\django-to-do-app> docker pull 301988/django-todo-app
Using default tag: latest
latest: Pulling from 301988/django-todo-app
206356c42440: Pull complete
bae47d04c81d: Pull complete
275d1bfbcde1: Pull complete
6e7d38d9cbfa: Pull complete
206709444425: Pull complete
bbf418262cbe: Pull complete
43272e9c9d4b: Pull complete
Digest: sha256:81c9cb5adf210413ce19492ebcc1f2eb8b60468271cce8a1d2b56afe169a6de8
Status: Downloaded newer image for 301988/django-todo-app:latest
docker.io/301988/django-todo-app:latest
PS D:\ANDYtws\twstask\day36\django-to-do-app> docker images
REPOSITORY               TAG       IMAGE ID       CREATED       SIZE
301988/django-todo-app   latest    81c9cb5adf21   2 hours ago   343MB
PS D:\ANDYtws\twstask\day36\django-to-do-app>
3. Yes it works 

'''
---




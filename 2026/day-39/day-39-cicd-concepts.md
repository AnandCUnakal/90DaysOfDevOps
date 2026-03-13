---

## Challenge Tasks

### Task 1: The Problem
Think about a team of 5 developers all pushing code to the same repo manually deploying to production.

Write in your notes:
1. What can go wrong?
2. What does "it works on my machine" mean and why is it a real problem?
3. How many times a day can a team safely deploy manually?

---

### Task 2: CI vs CD
Research and write short definitions (2-3 lines each):
1. **Continuous Integration** — what happens, how often, what it catches
2. **Continuous Delivery** — how it's different from CI, what "delivery" means
3. **Continuous Deployment** — how it differs from Delivery, when teams use it

Write one real-world example for each.

---

### Task 3: Pipeline Anatomy
A pipeline has these parts — write what each one does:
- **Trigger** — what starts the pipeline
- **Stage** — a logical phase (build, test, deploy)
- **Job** — a unit of work inside a stage
- **Step** — a single command or action inside a job
- **Runner** — the machine that executes the job
- **Artifact** — output produced by a job

---

### Task 4: Draw a Pipeline
Draw a CI/CD pipeline for this scenario:
> A developer pushes code to GitHub. The app is tested, built into a Docker image, and deployed to a staging server.

Include at least 3 stages. Hand-drawn and photographed is perfectly fine.

---

### Task 5: Explore in the Wild
1. Open any popular open-source repo on GitHub (Kubernetes, React, FastAPI — pick one you know)
2. Find their `.github/workflows/` folder
3. Open one workflow YAML file
4. Write in your notes:
   - What triggers it?
   - How many jobs does it have?
   - What does it do? (best guess)

---

'''
Below are **clear notes-style answers** you can write for each task.

---

# Task 1: The Problem

### 1. What can go wrong?

When 5 developers manually deploy to production:

* **Conflicts** – developers may deploy different versions of code.
* **Human errors** – wrong commands, wrong branch, or wrong environment.
* **Unstable production** – untested code may break the application.
* **Deployment overwrites** – one developer’s deployment may overwrite another’s.
* **No audit trail** – difficult to track who deployed what.

---

### 2. What does "It works on my machine" mean?

"It works on my machine" means:

* The code runs correctly on a developer's **local environment**
* But fails on **another machine or server**

Reasons this happens:

* Different OS
* Different library versions
* Missing dependencies
* Environment variable differences

Why it is a real problem:

* Causes **unexpected production failures**
* Makes debugging difficult
* Slows down development teams

This is why **Docker and CI pipelines** are widely used.

---

### 3. How many times a day can a team safely deploy manually?

Manual deployments are risky.

Typically teams deploy:

* **1–2 times per day safely**
* Sometimes **once every few days**

Reason:

* Manual testing and verification takes time.
* Human errors increase with frequent deployments.

Modern teams use **CI/CD pipelines** to deploy **dozens or hundreds of times per day** safely.

---

# Task 2: CI vs CD

## 1. Continuous Integration (CI)

Continuous Integration is the practice where developers **frequently merge code into a shared repository**, and automated pipelines **build and test the code**.

This usually happens **multiple times per day**.

CI helps catch:

* build failures
* test failures
* integration issues

Example:

A developer pushes code to GitHub → pipeline runs **build + unit tests automatically**.

---

## 2. Continuous Delivery

Continuous Delivery ensures that the code is **always in a deployable state**.

After CI completes:

* the application is **built**
* tested
* packaged

But **deployment to production requires manual approval**.

Example:

Code passes tests → Docker image built → ready to deploy → **release manager approves production deployment**.

---

## 3. Continuous Deployment

Continuous Deployment goes one step further.

After tests pass:

* code is **automatically deployed to production** with **no manual approval**.

Example:

Developer pushes code → pipeline runs → tests pass → application **automatically deployed to production servers**.

Companies like **Netflix, Amazon, and Facebook** use this approach.

---

# Task 3: Pipeline Anatomy

### Trigger

A **trigger** starts the pipeline.

Examples:

* Git push
* Pull request
* Scheduled job
* Manual trigger

Example:

```text
git push → pipeline starts
```

---

### Stage

A **stage** is a logical phase in the pipeline.

Common stages:

* Build
* Test
* Deploy

Example pipeline stages:

```text
Build → Test → Deploy
```

---

### Job

A **job** is a set of tasks executed in a stage.

Example:

Test stage might have jobs like:

```text
unit-test
integration-test
security-scan
```

---

### Step

A **step** is a **single command or action** inside a job.

Example:

```text
npm install
npm test
docker build
```

---

### Runner

A **runner** is the machine that executes pipeline jobs.

Examples:

* GitHub Actions runner
* GitLab runner
* Jenkins agent

It can be:

* a VM
* a container
* a physical machine

---

### Artifact

An **artifact** is the output produced by a job.

Examples:

* compiled binaries
* Docker images
* test reports
* build packages

Artifacts can be passed to the **next stage**.

---

# Task 4: Example CI/CD Pipeline

Scenario:

> Developer pushes code → test → build Docker image → deploy to staging.

Pipeline flow:

```
Developer Push
      ↓
Trigger (GitHub push event)
      ↓
Stage 1: Build
      Job: Install dependencies
      Job: Build application
      ↓
Stage 2: Test
      Job: Run unit tests
      Job: Run integration tests
      ↓
Stage 3: Docker Build
      Job: Build Docker image
      Job: Push image to registry
      ↓
Stage 4: Deploy
      Job: Deploy container to staging server
```

Simplified diagram:

```
Push Code
   ↓
Build
   ↓
Test
   ↓
Docker Build
   ↓
Deploy to Staging
```

---

# Task 5: Explore in the Wild

Example repository: **FastAPI**

Location:

```
.github/workflows/
```

Example workflow file:

```
ci.yml
```

### 1. What triggers it?

Common triggers:

```
push
pull_request
```

Example:

```yaml
on:
  push:
    branches: [main]
  pull_request:
```

Meaning:

Pipeline runs when code is **pushed or a PR is created**.

---

### 2. How many jobs does it have?

Typical CI workflow might contain jobs like:

```
lint
test
build
```

Example:

```
jobs:
  lint
  test
```

So it may have **2–3 jobs**.

---

### 3. What does it do? (best guess)

Typical workflow steps:

* install dependencies
* run code formatting checks
* run tests
* verify build

Purpose:

Ensure that **new code does not break the project**.

---
'''

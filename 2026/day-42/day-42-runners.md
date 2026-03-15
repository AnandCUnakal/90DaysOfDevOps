### Task 1: GitHub-Hosted Runners
1. Create a workflow with 3 jobs, each on a different OS:
   - `ubuntu-latest`
   - `windows-latest`
   - `macos-latest`
2. In each job, print:
   - The OS name
   - The runner's hostname
   - The current user running the job
3. Watch all 3 run in parallel

Write in your notes: What is a GitHub-hosted runner? Who manages it?

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3f6b2df0-cc9d-4919-bc3c-74133f574f6d" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/cf16155d-1985-4a5e-86e7-5f2bd29604ec" />

---

### Task 2: Explore What's Pre-installed
1. On the `ubuntu-latest` runner, run a step that prints:
   - Docker version
   - Python version
   - Node version
   - Git version
2. Look up the GitHub docs for the full list of pre-installed software on `ubuntu-latest`

Write in your notes: Why does it matter that runners come with tools pre-installed?

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/94ed5dad-363f-4d37-82a1-453bb757ff29" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/12efe26a-fccd-491a-94f5-78183a7a167d" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/99db99d8-56b6-49ba-86ce-c2231a0f79c4" />

---

### Task 3: Set Up a Self-Hosted Runner
1. Go to your GitHub repo → Settings → Actions → Runners → **New self-hosted runner**
2. Choose Linux as the OS
3. Follow the instructions to download and configure the runner on:
   - Your local machine, OR
   - A cloud VM (EC2, Utho, or any VPS)
4. Start the runner — verify it shows as **Idle** in GitHub

**Verify:** Your runner appears in the Runners list with a green dot.

---

### Task 4: Use Your Self-Hosted Runner
1. Create `.github/workflows/self-hosted.yml`
2. Set `runs-on: self-hosted`
3. Add steps that:
   - Print the hostname of the machine (it should be YOUR machine/VM)
   - Print the working directory
   - Create a file and verify it exists on your machine after the run
4. Trigger it and watch it run on your own hardware

**Verify:** Check your machine — is the file there?

---

### Task 5: Labels
1. Add a **label** to your self-hosted runner (e.g., `my-linux-runner`)
2. Update your workflow to use `runs-on: [self-hosted, my-linux-runner]`
3. Trigger it — does it still pick up the job?

Write in your notes: Why are labels useful when you have multiple self-hosted runners?

---

### Task 6: GitHub-Hosted vs Self-Hosted
Fill this in your notes:

| | GitHub-Hosted | Self-Hosted |
|---|---|---|
| Who manages it? | ? | ? |
| Cost | ? | ? |
| Pre-installed tools | ? | ? |
| Good for | ? | ? |
| Security concern | ? | ? |

---


'''
Below are **working workflow examples + the notes answers** for each task.

---

# Task 1: GitHub-Hosted Runners

Create:

```
.github/workflows/os-runners.yml
```

```yaml
name: Runner OS Test

on: push

jobs:
  ubuntu-job:
    runs-on: ubuntu-latest
    steps:
      - name: Print system info
        run: |
          echo "OS: $(uname -a)"
          echo "Hostname: $(hostname)"
          echo "User: $(whoami)"

  windows-job:
    runs-on: windows-latest
    steps:
      - name: Print system info
        run: |
          echo OS: %OS%
          hostname
          echo User: %USERNAME%

  macos-job:
    runs-on: macos-latest
    steps:
      - name: Print system info
        run: |
          echo "OS: $(uname -a)"
          echo "Hostname: $(hostname)"
          echo "User: $(whoami)"
```

When you push code:

```
ubuntu job
windows job
macos job
```

all run **in parallel**.

---

### Notes Answer

**What is a GitHub-hosted runner?**

A GitHub-hosted runner is a **temporary virtual machine provided by GitHub** that executes workflow jobs.

**Who manages it?**

GitHub manages:

* the infrastructure
* the OS
* updates
* pre-installed tools

The VM is **created at job start and destroyed afterward**.

---

# Task 2: Explore Pre-installed Tools

Add this job:

```yaml
jobs:
  tools-check:
    runs-on: ubuntu-latest

    steps:
      - name: Check installed tools
        run: |
          docker --version
          python --version
          node --version
          git --version
```

Example output:

```
Docker version 24.x
Python 3.11.x
Node v20.x
git version 2.x
```

---

### Notes Answer

**Why does pre-installed software matter?**

Because it:

* **reduces pipeline setup time**
* **speeds up CI builds**
* avoids repeatedly installing dependencies
* keeps environments **consistent across runs**

Without this, pipelines would spend minutes installing tools each run.

---

# Task 3: Self-Hosted Runner

Steps:

1. Go to your repo:

```
Settings → Actions → Runners
```

2. Click:

```
New self-hosted runner
```

3. Choose **Linux**

GitHub gives commands like:

```
mkdir actions-runner
cd actions-runner
```

Download runner:

```
curl -o actions-runner.tar.gz -L https://github.com/actions/runner/releases/latest/download/actions-runner-linux-x64.tar.gz
```

Extract:

```
tar xzf ./actions-runner.tar.gz
```

Configure runner:

```
./config.sh --url https://github.com/<username>/<repo> --token <token>
```

Start runner:

```
./run.sh
```

Now GitHub will show:

```
Self-hosted runner
Status: Idle
🟢 green dot
```

---

# Task 4: Use Your Self-Hosted Runner

Create:

```
.github/workflows/self-hosted.yml
```

```yaml
name: Self Hosted Test

on: push

jobs:
  test:
    runs-on: self-hosted

    steps:
      - name: Print hostname
        run: hostname

      - name: Print working directory
        run: pwd

      - name: Create file
        run: |
          echo "Hello from GitHub runner" > runner-test.txt
          ls
```

After pipeline runs:

Check your machine:

```
runner-test.txt
```

It should exist in the runner working directory.

This proves the job ran **on your machine**.

---

# Task 5: Labels

Add label in:

```
Settings → Actions → Runners
```

Example label:

```
my-linux-runner
```

Update workflow:

```yaml
runs-on: [self-hosted, my-linux-runner]
```

Example:

```yaml
jobs:
  test:
    runs-on: [self-hosted, my-linux-runner]

    steps:
      - run: echo "Running on my labeled runner"
```

Trigger workflow.

It will run **only on runners with that label**.

---

### Notes Answer

**Why are labels useful?**

When you have many runners.

Example setup:

```
runner-1 → gpu
runner-2 → large-memory
runner-3 → docker
```

You can target specific hardware.

Example:

```
runs-on: [self-hosted, gpu]
```

This ensures the job runs on the **GPU runner**.

---

# Task 6: GitHub-Hosted vs Self-Hosted

|                     | GitHub-Hosted                      | Self-Hosted                                 |
| ------------------- | ---------------------------------- | ------------------------------------------- |
| Who manages it?     | GitHub                             | You / your team                             |
| Cost                | Free minutes or paid usage         | Infrastructure cost (VM/server)             |
| Pre-installed tools | Many tools preinstalled            | You must install them                       |
| Good for            | Simple CI pipelines                | Custom environments / large workloads       |
| Security concern    | Code runs on GitHub infrastructure | Risk if untrusted code runs on your machine |

---

# Real DevOps Insight

Most companies use **both**:

```
GitHub-hosted → CI tests
Self-hosted → builds, Docker, deployments
```

Example:

```
GitHub runner → run tests
Self-hosted runner → build Docker image
Self-hosted runner → deploy to Kubernetes
```

---

If you want, I can also show you **the real CI/CD pipeline most DevOps engineers build next:**

```
GitHub Actions
   ↓
Build Docker image
   ↓
Push to Docker Hub
   ↓
Deploy to Kubernetes
```

This is the **next real-world milestone after learning runners.**

'''

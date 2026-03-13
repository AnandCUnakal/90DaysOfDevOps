### Task 1: Set Up
1. Create a new **public** GitHub repository called `github-actions-practice`
2. Clone it locally
3. Create the folder structure: `.github/workflows/`


'''

'''
---

### Task 2: Hello Workflow
Create `.github/workflows/hello.yml` with a workflow that:
1. Triggers on every `push`
2. Has one job called `greet`
3. Runs on `ubuntu-latest`
4. Has two steps:
   - Step 1: Check out the code using `actions/checkout`
   - Step 2: Print `Hello from GitHub Actions!`

Push it. Go to the **Actions** tab on GitHub and watch it run.

**Verify:** Is it green? Click into the job and read every step.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a0573edd-ccf9-48c5-a9fc-e39a67821dec" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/eb864abd-2669-448a-b893-bc09daddebfb" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/95dd288a-1b83-4d75-981e-d82814a57503" />


---

### Task 3: Understand the Anatomy
Look at your workflow file and write in your notes what each key does:
- `on:`
- `jobs:`
- `runs-on:`
- `steps:`
- `uses:`
- `run:`
- `name:` (on a step)


'''
Here are **clear notes-style explanations** for the GitHub Actions workflow keys.

---

# Task 3: Understand the Anatomy (GitHub Actions)

## 1️⃣ `on:`

`on:` defines **what event triggers the workflow**.

Examples of triggers:

* `push`
* `pull_request`
* `schedule`
* `workflow_dispatch` (manual run)

Example:

```yaml
on:
  push:
    branches: [main]
```

Meaning:

```text
When code is pushed to the main branch → start the workflow
```

---

## 2️⃣ `jobs:`

`jobs:` defines **the set of tasks the workflow will run**.

A workflow can contain **multiple jobs**.

Example:

```yaml
jobs:
  build:
  test:
  deploy:
```

Meaning:

```text
The pipeline will execute build, test, and deploy jobs.
```

Each job runs **independently on a runner**.

---

## 3️⃣ `runs-on:`

`runs-on:` specifies **the type of machine that executes the job**.

Example:

```yaml
runs-on: ubuntu-latest
```

Meaning:

```text
The job runs on a GitHub-hosted Ubuntu virtual machine.
```

Other examples:

```text
windows-latest
macos-latest
self-hosted
```

---

## 4️⃣ `steps:`

`steps:` defines the **sequence of tasks executed inside a job**.

Each step performs a **single action**.

Example:

```yaml
steps:
  - name: Checkout code
    uses: actions/checkout@v4
```

Meaning:

```text
Steps are the ordered commands executed in the job.
```

---

## 5️⃣ `uses:`

`uses:` allows a workflow to **reuse existing GitHub Actions**.

These actions are reusable automation blocks.

Example:

```yaml
uses: actions/checkout@v4
```

Meaning:

```text
Use the checkout action to clone the repository code.
```

Another example:

```yaml
uses: actions/setup-node@v4
```

Used to **install Node.js** in the runner.

---

## 6️⃣ `run:`

`run:` executes **shell commands directly** on the runner machine.

Example:

```yaml
run: npm install
```

Example with multiple commands:

```yaml
run: |
  npm install
  npm test
```

Meaning:

```text
Execute shell commands inside the pipeline step.
```

---

## 7️⃣ `name:` (on a step)

`name:` provides a **human-readable label for a step**.

It helps make the pipeline **easier to read in the UI**.

Example:

```yaml
- name: Install dependencies
  run: npm install
```

In the GitHub Actions interface it will display:

```text
✔ Install dependencies
```

---

# Example Workflow Putting It All Together

```yaml
name: CI Pipeline

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test
```

Pipeline flow:

```text
Push Code
   ↓
GitHub Actions Trigger
   ↓
Job runs on Ubuntu VM
   ↓
Step 1: Checkout repo
Step 2: Install dependencies
Step 3: Run tests
```

---
'''
---

### Task 4: Add More Steps
Update `hello.yml` to also:
1. Print the current date and time
2. Print the name of the branch that triggered the run (hint: GitHub provides this as a variable)
3. List the files in the repo
4. Print the runner's operating system

Push again — watch the new run.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a121e84b-b3c1-4f0b-a15c-dae011636795" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fe819e3b-bc8b-4f63-b0c5-28595d123589" />

---

### Task 5: Break It On Purpose
1. Add a step that runs a command that will **fail** (e.g., `exit 1` or a misspelled command)
2. Push and observe what happens in the Actions tab
3. Fix it and push again

Write in your notes: What does a failed pipeline look like? How do you read the error?

Job Failed 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/129040d7-79b0-4d03-bcc6-55bebb07c2b8" />
After making chabges then its worked
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6c5a610f-bf54-429f-bba2-75bd06dc4cb9" />


---

---

## Challenge Tasks

### Task 1: Trigger on Pull Request
1. Create `.github/workflows/pr-check.yml`
2. Trigger it only when a pull request is **opened or updated** against `main`
3. Add a step that prints: `PR check running for branch: <branch name>`
4. Create a new branch, push a commit, and open a PR
5. Watch the workflow run automatically

**Verify:** Does it show up on the PR page?

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b52ec315-bea9-4948-8447-b887be7aa689" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/655c0818-1b23-4de5-9320-23e6e47dd0a2" />

---

### Task 2: Scheduled Trigger
1. Add a `schedule:` trigger to any workflow using cron syntax
2. Set it to run every day at midnight UTC
3. Write in your notes: What is the cron expression for every Monday at 9 AM?

'''
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7de24b7e-4334-416d-a3f3-99d2082e8ba2" />

'''
---

### Task 3: Manual Trigger
1. Create `.github/workflows/manual.yml` with a `workflow_dispatch:` trigger
2. Add an **input** that asks for an `environment` name (staging/production)
3. Print the input value in a step
4. Go to the **Actions** tab → find the workflow → click **Run workflow**

**Verify:** Can you trigger it manually and see your input printed?
'''
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/08a225e5-44b3-4b9b-99f5-44bc12fce027" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/97a6dd05-f334-40e5-a5a9-f3a39daec033" />

'''
---

### Task 4: Matrix Builds
Create `.github/workflows/matrix.yml` that:
1. Uses a matrix strategy to run the same job across:
   - Python versions: `3.10`, `3.11`, `3.12`
2. Each job installs Python and prints the version
3. Watch all 3 run in parallel

Then extend the matrix to also include 2 operating systems — how many total jobs run now?

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f4b19bd6-76c5-4aad-b91b-32c995990363" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ed824dda-717e-4f1f-be8e-8f2f611a2caa" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e2981049-8967-4981-945d-bca7624d8599" />

---

### Task 5: Exclude & Fail-Fast
1. In your matrix, **exclude** one specific combination (e.g., Python 3.10 on Windows)
2. Set `fail-fast: false` — trigger a failure in one job and observe what happens to the rest
3. Write in your notes: What does `fail-fast: true` (the default) do vs `false`?
   
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f6e9a7ee-d71a-4f7a-bb36-0c11ec509ed5" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9ee7fd93-4789-4925-bbb9-0871988991cc" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8ab7957e-aad5-4277-9633-e88fe4aeaf8a" />

---


'''
Below are **working GitHub Actions examples** for each challenge task plus the answers you should write in your notes.

---

# Task 1: Trigger on Pull Request

Create:

```
.github/workflows/pr-check.yml
```

```yaml
name: PR Check

on:
  pull_request:
    branches:
      - main
    types: [opened, synchronize]

jobs:
  pr-check:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Print PR branch
        run: echo "PR check running for branch: ${{ github.head_ref }}"
```

### What the trigger means

```
opened       → when PR is created
synchronize  → when new commits are pushed to the PR
```

### Test it

1. Create a branch

```bash
git checkout -b feature-test
```

2. Push code

```bash
git push origin feature-test
```

3. Open PR → target **main**

✅ The workflow will appear on the **PR page under Checks**.

---

# Task 2: Scheduled Trigger

Add to any workflow:

```yaml
on:
  schedule:
    - cron: "0 0 * * *"
```

### Meaning

```
0 0 * * *  → every day at midnight UTC
```

---

### Answer for your notes

Cron for **every Monday at 9 AM**

```
0 9 * * 1
```

Cron format:

```
┌ minute
│ ┌ hour
│ │ ┌ day
│ │ │ ┌ month
│ │ │ │ ┌ day of week
│ │ │ │ │
* * * * *
```

---

# Task 3: Manual Trigger

Create:

```
.github/workflows/manual.yml
```

```yaml
name: Manual Deployment

on:
  workflow_dispatch:
    inputs:
      environment:
        description: "Choose environment"
        required: true
        default: "staging"

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Print selected environment
        run: echo "Deploying to ${{ github.event.inputs.environment }}"
```

### Run manually

1. Go to **GitHub → Actions**
2. Select workflow
3. Click **Run workflow**
4. Enter:

```
staging
```

or

```
production
```

Example output:

```
Deploying to production
```

---

# Task 4: Matrix Builds

Create:

```
.github/workflows/matrix.yml
```

```yaml
name: Python Matrix

on:
  push:

jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        python-version: [3.10, 3.11, 3.12]

    steps:
      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}

      - name: Print Python version
        run: python --version
```

### Result

Runs **3 jobs in parallel**:

```
Python 3.10
Python 3.11
Python 3.12
```

---

### Extend Matrix with OS

```yaml
strategy:
  matrix:
    os: [ubuntu-latest, windows-latest]
    python-version: [3.10, 3.11, 3.12]
```

Update runner:

```yaml
runs-on: ${{ matrix.os }}
```

### Total Jobs

```
3 Python versions × 2 OS = 6 jobs
```

---

# Task 5: Exclude & Fail-Fast

### Example Matrix

```yaml
strategy:
  fail-fast: false

  matrix:
    os: [ubuntu-latest, windows-latest]
    python-version: [3.10, 3.11, 3.12]

    exclude:
      - os: windows-latest
        python-version: 3.10
```

This removes:

```
Windows + Python 3.10
```

---

### What happens with `fail-fast`

Default:

```
fail-fast: true
```

If one matrix job fails:

```
Remaining jobs are cancelled immediately
```

---

With:

```
fail-fast: false
```

If one job fails:

```
Other jobs continue running
```

This is useful when you want **full test results across all environments**.

---

# Example Matrix Result

After exclusion:

```
Ubuntu 3.10
Ubuntu 3.11
Ubuntu 3.12
Windows 3.11
Windows 3.12
```

Total jobs:

```
5 jobs
```

---

# Real DevOps Insight

Matrix builds are heavily used for:

```
Testing multiple Python versions
Testing Node versions
Testing across Linux/Windows/macOS
Cross-platform builds
```

Large projects like:

* Python libraries
* Kubernetes tools
* Terraform providers

use matrix builds extensively.

---
'''

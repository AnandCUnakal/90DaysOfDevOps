### Task 1: GitHub Secrets
1. Go to your repo → Settings → Secrets and Variables → Actions
2. Create a secret called `MY_SECRET_MESSAGE`
3. Create a workflow that reads it and prints: `The secret is set: true` (never print the actual value)
4. Try to print `${{ secrets.MY_SECRET_MESSAGE }}` directly — what does GitHub show?

Write in your notes: Why should you never print secrets in CI logs?
https://github.com/AnandCUnakal/github-actions-ero-to-hero/actions/runs/23116290309/job/67142290153
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7544af1f-6ef3-4c2c-b164-f5d0d1b4bd0b" />

---

### Task 2: Use Secrets as Environment Variables
1. Pass a secret to a step as an environment variable
2. Use it in a shell command without ever hardcoding it
3. Add `DOCKER_USERNAME` and `DOCKER_TOKEN` as secrets (you'll need these on Day 45)

'''
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/dd42311f-38f0-46c4-b56f-dfb7f4c2ad5d" />

'''
---

### Task 3: Upload Artifacts
1. Create a step that generates a file — e.g., a test report or a log file
2. Use `actions/upload-artifact` to save it
3. After the workflow runs, download the artifact from the Actions tab

**Verify:** Can you see and download it from GitHub?
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/60088846-9da6-4803-91cb-7b104f43fa25" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b56625b0-4ade-4fe6-8f2e-312d056d5a96" />

---

### Task 4: Download Artifacts Between Jobs
1. Job 1: generate a file and upload it as an artifact
2. Job 2: download the artifact from Job 1 and use it (print its contents)

Write in your notes: When would you use artifacts in a real pipeline?

---

### Task 5: Run Real Tests in CI
Take any script from your earlier days (Python or Shell) and run it in CI:
1. Add your script to the `github-actions-practice` repo
2. Write a workflow that:
   - Checks out the code
   - Installs any dependencies needed
   - Runs the script
   - Fails the pipeline if the script exits with a non-zero code
3. Intentionally break the script — verify the pipeline goes red
4. Fix it — verify it goes green again

---


'''
Below are **practical solutions + example workflows + notes answers** for each task.

---

# Task 1: GitHub Secrets

### Step 1 — Create Secret

Go to:

```
Repo → Settings → Secrets and variables → Actions
```

Click **New repository secret**

Example:

```
Name: MY_SECRET_MESSAGE
Value: HelloFromSecret
```

---

### Workflow to check if secret exists

Create:

```text
.github/workflows/secrets-demo.yml
```

```yaml
name: Secrets Demo

on: push

jobs:
  check-secret:
    runs-on: ubuntu-latest

    steps:
      - name: Check if secret exists
        run: |
          if [ -n "${{ secrets.MY_SECRET_MESSAGE }}" ]; then
            echo "The secret is set: true"
          else
            echo "The secret is missing"
          fi
```

Output:

```
The secret is set: true
```

---

### What happens if you print the secret directly?

Example step:

```yaml
- run: echo "${{ secrets.MY_SECRET_MESSAGE }}"
```

GitHub output:

```
***
```

GitHub **masks the value automatically**.

---

### Notes Answer

**Why should you never print secrets in CI logs?**

Because:

• Logs may be publicly visible
• Anyone with repo access can read them
• Secrets could leak credentials (API keys, tokens, passwords)

If leaked:

```
Attackers can access your infrastructure.
```

---

# Task 2: Use Secrets as Environment Variables

Example workflow:

```yaml
name: Secret Environment Variable

on: push

jobs:
  use-secret:
    runs-on: ubuntu-latest

    steps:
      - name: Use secret safely
        env:
          SECRET_MESSAGE: ${{ secrets.MY_SECRET_MESSAGE }}
        run: |
          echo "Using secret internally"
          echo "Length of secret: ${#SECRET_MESSAGE}"
```

Notice:

```
We never print the actual secret value
```

---

### Add Docker Secrets (important later)

Create two secrets:

```
DOCKER_USERNAME
DOCKER_TOKEN
```

These will be used for:

```
docker login
docker push
```

Example:

```yaml
env:
  DOCKER_USERNAME: ${{ secrets.DOCKER_USERNAME }}
  DOCKER_TOKEN: ${{ secrets.DOCKER_TOKEN }}
```

---

# Task 3: Upload Artifacts

Create a workflow:

```text
.github/workflows/artifact-upload.yml
```

```yaml
name: Upload Artifact

on: push

jobs:
  create-artifact:
    runs-on: ubuntu-latest

    steps:
      - name: Generate report
        run: |
          echo "Test Report" > report.txt
          date >> report.txt
          cat report.txt

      - name: Upload artifact
        uses: actions/upload-artifact@v4
        with:
          name: test-report
          path: report.txt
```

After the workflow runs:

Go to:

```
Actions → Workflow run → Artifacts
```

Download **test-report.zip**

---

# Task 4: Download Artifacts Between Jobs

Create:

```text
.github/workflows/artifact-transfer.yml
```

```yaml
name: Artifact Transfer

on: push

jobs:
  create-file:
    runs-on: ubuntu-latest

    steps:
      - name: Create file
        run: |
          echo "Hello from Job 1" > message.txt

      - name: Upload file
        uses: actions/upload-artifact@v4
        with:
          name: shared-file
          path: message.txt

  use-file:
    runs-on: ubuntu-latest
    needs: create-file

    steps:
      - name: Download artifact
        uses: actions/download-artifact@v4
        with:
          name: shared-file

      - name: Print file
        run: cat message.txt
```

Output:

```
Hello from Job 1
```

---

### Notes Answer

**When are artifacts used in real pipelines?**

Examples:

| Use Case              | Example                    |
| --------------------- | -------------------------- |
| Build output          | compiled binaries          |
| Test reports          | junit / coverage reports   |
| Docker image metadata | image tags                 |
| Security scan reports | vulnerability reports      |
| Logs                  | debugging failed pipelines |

Example pipeline:

```
build job → artifact = application package
test job → uses artifact
deploy job → deploys artifact
```

---

# Task 5: Run Real Tests in CI

Example project structure:

```
github-actions-practice
│
├── scripts
│   └── test.sh
└── .github/workflows/run-tests.yml
```

---

### Example Script

`scripts/test.sh`

```bash
#!/bin/bash

echo "Running tests..."

if [ 2 -eq 2 ]; then
  echo "Tests passed"
  exit 0
else
  echo "Tests failed"
  exit 1
fi
```

Make executable:

```
chmod +x scripts/test.sh
```

---

### Workflow

```yaml
name: Run Tests

on: push

jobs:
  test-script:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Run test script
        run: |
          chmod +x scripts/test.sh
          ./scripts/test.sh
```

---

### Break the Script

Change:

```
if [ 2 -eq 2 ]
```

to

```
if [ 2 -eq 3 ]
```

Pipeline result:

```
❌ Failed
```

Fix it again → pipeline becomes:

```
✅ Passed
```

---

# Key DevOps Skills You Just Learned

You now understand **production CI concepts**:

| Feature          | Purpose                  |
| ---------------- | ------------------------ |
| Secrets          | secure credentials       |
| Artifacts        | share files between jobs |
| Job dependencies | workflow order           |
| CI testing       | prevent broken code      |
| Secure variables | protect infrastructure   |

---

# Real Production Example Pipeline

A typical **DevOps CI pipeline** looks like:

```
Developer Push
     ↓
Lint Code
     ↓
Run Unit Tests
     ↓
Build Application
     ↓
Upload Artifact
     ↓
Build Docker Image
     ↓
Push to Docker Registry
     ↓
Deploy to Kubernetes
```

---

'''

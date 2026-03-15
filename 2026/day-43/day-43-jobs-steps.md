### Task 1: Multi-Job Workflow
Create `.github/workflows/multi-job.yml` with 3 jobs:
- `build` — prints "Building the app"
- `test` — prints "Running tests"
- `deploy` — prints "Deploying"

Make `test` run only **after** `build` succeeds.
Make `deploy` run only **after** `test` succeeds.

**Verify:** Check the workflow graph in the Actions tab — does it show the dependency chain?

---

### Task 2: Environment Variables
In a new workflow, use environment variables at 3 levels:
1. **Workflow level** — `APP_NAME: myapp`
2. **Job level** — `ENVIRONMENT: staging`
3. **Step level** — `VERSION: 1.0.0`

Print all three in a single step and verify each is accessible.

Then use a **GitHub context variable** — print the commit SHA and the actor (who triggered the run).

---

### Task 3: Job Outputs
1. Create a job that **sets an output** — e.g., today's date as a string
2. Create a second job that **reads that output** and prints it
3. Pass the value using `outputs:` and `needs.<job>.outputs.<name>`

Write in your notes: Why would you pass outputs between jobs?

---

### Task 4: Conditionals
In a workflow, add:
1. A step that only runs when the branch is `main`
2. A step that only runs when the previous step **failed**
3. A job that only runs on **push** events, not on pull requests
4. A step with `continue-on-error: true` — what does this do?

---

### Task 5: Putting It Together
Create `.github/workflows/smart-pipeline.yml` that:
1. Triggers on push to any branch
2. Has a `lint` job and a `test` job running in parallel
3. Has a `summary` job that runs after both, prints whether it's a `main` branch push or a feature branch push, and prints the commit message

---


'''
Below are **working GitHub Actions workflows for each task** plus the key notes you should write.

---

# Task 1: Multi-Job Workflow

Create:

```text
.github/workflows/multi-job.yml
```

```yaml
name: Multi Job Pipeline

on: push

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Building the app"

  test:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - run: echo "Running tests"

  deploy:
    runs-on: ubuntu-latest
    needs: test
    steps:
      - run: echo "Deploying"
```

### Dependency Chain

```
build → test → deploy
```

Open **Actions → Workflow run → Graph**
You should see the **job dependency chain**.

---

# Task 2: Environment Variables

Create:

```text
.github/workflows/env-vars.yml
```

```yaml
name: Environment Variables Demo

on: push

env:
  APP_NAME: myapp

jobs:
  env-test:
    runs-on: ubuntu-latest

    env:
      ENVIRONMENT: staging

    steps:
      - name: Print variables
        env:
          VERSION: 1.0.0
        run: |
          echo "App: $APP_NAME"
          echo "Environment: $ENVIRONMENT"
          echo "Version: $VERSION"

          echo "Commit SHA: ${{ github.sha }}"
          echo "Triggered by: ${{ github.actor }}"
```

### Variable Levels

| Level    | Scope                           |
| -------- | ------------------------------- |
| Workflow | available everywhere            |
| Job      | available only inside that job  |
| Step     | available only inside that step |

---

# Task 3: Job Outputs

Create:

```text
.github/workflows/job-outputs.yml
```

```yaml
name: Job Outputs Example

on: push

jobs:
  generate-date:
    runs-on: ubuntu-latest
    outputs:
      today: ${{ steps.setdate.outputs.today }}

    steps:
      - id: setdate
        run: echo "today=$(date)" >> $GITHUB_OUTPUT

  use-date:
    runs-on: ubuntu-latest
    needs: generate-date

    steps:
      - run: echo "Date from previous job: ${{ needs.generate-date.outputs.today }}"
```

### Notes Answer

Why pass outputs between jobs?

Because jobs run on **different runners**.
Outputs allow you to **share data between jobs** such as:

* build version
* artifact location
* generated image tag
* timestamps

Example:

```
build job → produces Docker image tag
deploy job → uses the tag
```

---

# Task 4: Conditionals

Example workflow:

```yaml
name: Conditional Demo

on:
  push:
  pull_request:

jobs:
  example:
    runs-on: ubuntu-latest

    steps:
      - name: Run only on main branch
        if: github.ref == 'refs/heads/main'
        run: echo "This runs only on main"

      - name: Intentional failure
        id: failstep
        run: exit 1
        continue-on-error: true

      - name: Run if previous step failed
        if: failure()
        run: echo "Previous step failed!"
        
  push-only-job:
    runs-on: ubuntu-latest
    if: github.event_name == 'push'
    steps:
      - run: echo "This job runs only on push events"
```

### Notes Answer

**What does `continue-on-error: true` do?**

It allows the workflow to **continue even if the step fails**.

Without it:

```
Step fails → job stops
```

With it:

```
Step fails → job continues to next step
```

Useful for **optional checks or experiments**.

---

# Task 5: Smart Pipeline

Create:

```text
.github/workflows/smart-pipeline.yml
```

```yaml
name: Smart Pipeline

on:
  push:

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Running lint checks"

  test:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Running tests"

  summary:
    runs-on: ubuntu-latest
    needs: [lint, test]

    steps:
      - name: Branch info
        run: |
          if [[ "${{ github.ref_name }}" == "main" ]]; then
            echo "Main branch push"
          else
            echo "Feature branch push"
          fi

      - name: Print commit message
        run: echo "${{ github.event.head_commit.message }}"
```

### Pipeline Flow

```
Push Code
   ↓
lint       test   (parallel)
   ↓        ↓
      summary
```

---

# Important DevOps Concept Learned Here

You just used **core CI/CD concepts**:

| Feature               | Why It Matters           |
| --------------------- | ------------------------ |
| Job dependencies      | Control execution order  |
| Environment variables | Configuration management |
| Job outputs           | Pass data between jobs   |
| Conditionals          | Dynamic pipelines        |
| Parallel jobs         | Faster builds            |

---

💡 **Real DevOps Insight**

Most production pipelines look like this:

```
Push Code
   ↓
Lint + Security Scan
   ↓
Build Application
   ↓
Run Tests
   ↓
Build Docker Image
   ↓
Push to Registry
   ↓
Deploy to Staging
   ↓
Manual Approval
   ↓
Deploy to Production
```

---

If you want, I can also show you **the most important GitHub Actions pipeline DevOps engineers build next:**

**Build Docker Image → Push to Docker Hub → Deploy Container automatically.**

'''

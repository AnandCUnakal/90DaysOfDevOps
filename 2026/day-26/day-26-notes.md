---
### Task 1: Install and Authenticate
1. Install the GitHub CLI on your machine
2. Authenticate with your GitHub account
3. Verify you're logged in and check which account is active
4. Answer in your notes: What authentication methods does `gh` support?


---
Here are clean **notes-style answers** for Task 1 👇

---

# ✅ Task 1: Install and Authenticate GitHub CLI (`gh`)

GitHub CLI = official command-line tool from GitHub.

---

# 1️⃣ Install GitHub CLI

## 🔹 Windows (PowerShell)

```bash
winget install --id GitHub.cli
```

Or download from:
👉 [https://cli.github.com/](https://cli.github.com/)

---

## 🔹 macOS (Homebrew)

```bash
brew install gh
```

---

## 🔹 Linux (Ubuntu/Debian)

```bash
sudo apt install gh
```

---

## 🔹 Verify Installation

```bash
gh --version
```

---

# 2️⃣ Authenticate with GitHub

Run:

```bash
gh auth login
```

You’ll be prompted to choose:

* GitHub.com or GitHub Enterprise
* HTTPS or SSH
* Login via browser or token

Recommended for most users:

```
GitHub.com
HTTPS
Login with browser
```

Browser opens → Authorize → CLI stores credentials.

---

# 3️⃣ Verify Logged-in Account

```bash
gh auth status
```

Example output:

```
✓ Logged in to github.com as username
✓ Git operations for github.com configured to use https
✓ Token: ********
```

To check username:

```bash
gh api user
```

---

# 🧠 Notes Question

## ❓ What Authentication Methods Does `gh` Support?

GitHub CLI supports:

### 1️⃣ Browser-based OAuth (Recommended)

* Opens browser
* Secure
* Uses OAuth token

---

### 2️⃣ Personal Access Token (PAT)

```bash
gh auth login --with-token
```

* Manually paste token
* Used in CI/CD pipelines
* Common in automation

---

### 3️⃣ SSH Authentication

* Uses SSH key already configured
* Git operations use SSH protocol

---

### 4️⃣ GitHub Enterprise Authentication

* Supports enterprise server login
* Custom domain support

---

# 📌 Real DevOps Usage

In CI/CD pipelines (like Jenkins, GitLab, GitHub Actions):

* Use **PAT authentication**
* Store token in secure secret manager

Never hardcode tokens in code.

---

# 🎯 Interview-Level Answer

> GitHub CLI supports OAuth browser login, Personal Access Tokens, SSH authentication, and GitHub Enterprise authentication. OAuth is recommended for local use, while PAT is commonly used in automation and CI/CD environments.


---

---

### Task 2: Working with Repositories
1. Create a **new GitHub repo** directly from the terminal — make it public with a README
2. Clone a repo using `gh` instead of `git clone`
3. View details of one of your repos from the terminal
4. List all your repositories
5. Open a repo in your browser directly from the terminal
6. Delete the test repo you created (be careful!)

---
Here are clean **notes-style steps** for Task 2 👇

Using **GitHub CLI (`gh`)** from GitHub.

---

# ✅ Task 2: Working with Repositories using `gh`

---

# 1️⃣ Create a New Public Repo with README

```bash
gh repo create test-repo-cli \
  --public \
  --description "Test repo created from CLI" \
  --add-readme \
  --clone
```

### 🔎 What this does:

* Creates repo on GitHub
* Makes it public
* Adds a README.md
* Clones it locally automatically

If you don’t want auto-clone, remove `--clone`.

---

# 2️⃣ Clone a Repo Using `gh`

Instead of:

```bash
git clone <url>
```

Use:

```bash
gh repo clone username/repo-name
```

Example:

```bash
gh repo clone username/test-repo-cli
```

---

# 3️⃣ View Repo Details from Terminal

```bash
gh repo view username/test-repo-cli
```

To see more details:

```bash
gh repo view username/test-repo-cli --web
```

Shows:

* Description
* Visibility
* Default branch
* URL
* Stars/Forks

---

# 4️⃣ List All Your Repositories

```bash
gh repo list
```

Limit number:

```bash
gh repo list --limit 20
```

List only your repos:

```bash
gh repo list <your-username>
```

---

# 5️⃣ Open Repo in Browser from Terminal

```bash
gh repo view --web
```

Or:

```bash
gh browse
```

Browser opens directly to the repository page.

---

# 6️⃣ Delete the Test Repo (⚠ Be Careful)

```bash
gh repo delete username/test-repo-cli
```

You’ll be asked for confirmation.

To skip prompt:

```bash
gh repo delete username/test-repo-cli --confirm
```

⚠ This permanently deletes the repository from GitHub.

---

# 🧠 What You Learned

With `gh` you can:

* Create repositories
* Clone repos
* View repo info
* List repos
* Open in browser
* Delete repos

All without leaving the terminal.

---

# 🎯 Interview-Level Summary

> GitHub CLI allows full repository management from the terminal including creation, cloning, viewing, and deletion. It integrates with GitHub authentication and is useful for automation and DevOps workflows.

---

---
---

---

### Task 3: Issues
1. Create an issue on one of your repos from the terminal — give it a title, body, and a label
2. List all open issues on that repo
3. View a specific issue by its number
4. Close an issue from the terminal
5. Answer in your notes: How could you use `gh issue` in a script or automation?

---
Here are clean **notes-style steps** for Task 3 👇

Using **GitHub CLI (`gh`)** from GitHub.

---

# ✅ Task 3: Working with Issues using `gh`

Assume repo: `username/test-repo-cli`
(You can omit repo name if you're inside that repo directory.)

---

# 1️⃣ Create an Issue from Terminal

```bash
gh issue create \
  --title "Bug: Login page error" \
  --body "Users are getting 500 error while logging in. Needs investigation." \
  --label "bug"
```

If label doesn’t exist, create it first:

```bash
gh label create bug --description "Bug issues" --color FF0000
```

You can also assign someone:

```bash
gh issue create \
  --title "Fix CSS issue" \
  --body "Alignment problem in profile page." \
  --assignee @me
```

---

# 2️⃣ List All Open Issues

```bash
gh issue list
```

Options:

```bash
gh issue list --state all
gh issue list --label bug
gh issue list --assignee @me
```

---

# 3️⃣ View a Specific Issue by Number

```bash
gh issue view 1
```

Open in browser:

```bash
gh issue view 1 --web
```

---

# 4️⃣ Close an Issue from Terminal

```bash
gh issue close 1
```

Add closing comment:

```bash
gh issue close 1 --comment "Issue resolved in PR #5"
```

---

# 🧠 Notes Question

## ❓ How Could You Use `gh issue` in Script or Automation?

### 🔹 1️⃣ CI/CD Failure → Auto Create Issue

In pipeline (Jenkins/GitHub Actions):

```bash
gh issue create \
  --title "Build Failed in Production" \
  --body "Pipeline failed at deploy stage. Check logs." \
  --label "production"
```

👉 Automatically logs incidents.

---

### 🔹 2️⃣ Monitoring Alerts Integration

If monitoring tool detects outage:

* Script triggers `gh issue create`
* Creates incident tracking issue

---

### 🔹 3️⃣ Scheduled Maintenance Reminders

Cron job example:

```bash
gh issue create \
  --title "Monthly Security Patch Reminder" \
  --body "Run patching playbook."
```

---

### 🔹 4️⃣ Bulk Issue Management

Auto-close stale issues:

```bash
gh issue list --state open --limit 100 \
  | awk '{print $1}' \
  | xargs -I {} gh issue close {}
```

---

# 🎯 DevOps-Level Answer

> The `gh issue` command can be integrated into CI/CD pipelines, monitoring systems, or cron jobs to automatically create, update, or close issues. It enables automated incident tracking and workflow management directly from scripts.

---


---
---

---

### Task 4: Pull Requests
1. Create a branch, make a change, push it, and create a **pull request** entirely from the terminal
2. List all open PRs on a repo
3. View the details of your PR — check its status, reviewers, and checks
4. Merge your PR from the terminal
5. Answer in your notes:
   - What merge methods does `gh pr merge` support?
   - How would you review someone else's PR using `gh`?

---
Here are clean **notes-style steps** for Task 4 👇

Using **GitHub CLI (`gh`)** from GitHub.

---

# ✅ Task 4: Pull Requests using `gh`

Assume you are inside your repo directory.

---

# 1️⃣ Create Branch → Make Change → Push → Create PR

### 🔹 Create new branch

```bash
git checkout -b feature-cli-pr
```

---

### 🔹 Make a change

```bash
echo "New update from CLI PR" >> README.md
git add README.md
git commit -m "Update README via CLI"
```

---

### 🔹 Push branch

```bash
git push origin feature-cli-pr
```

---

### 🔹 Create Pull Request from Terminal

```bash
gh pr create \
  --title "Update README from CLI" \
  --body "This PR updates the README file." \
  --base main \
  --head feature-cli-pr
```

You can also assign reviewers:

```bash
gh pr create \
  --reviewer username \
  --assignee @me
```

---

# 2️⃣ List All Open PRs

```bash
gh pr list
```

More options:

```bash
gh pr list --state all
gh pr list --author @me
gh pr list --review-requested @me
```

---

# 3️⃣ View PR Details (Status, Reviewers, Checks)

```bash
gh pr view 1
```

To see full details including checks:

```bash
gh pr view 1 --json statusCheckRollup,reviewRequests,mergeStateStatus
```

Open in browser:

```bash
gh pr view 1 --web
```

This shows:

* PR status
* Reviewers
* CI checks
* Mergeability

---

# 4️⃣ Merge PR from Terminal

### 🔹 Regular merge

```bash
gh pr merge 1 --merge
```

---

### 🔹 Squash merge

```bash
gh pr merge 1 --squash
```

---

### 🔹 Rebase merge

```bash
gh pr merge 1 --rebase
```

Auto-delete branch:

```bash
gh pr merge 1 --merge --delete-branch
```

---

# 🧠 Notes Question

---

## ❓ What Merge Methods Does `gh pr merge` Support?

`gh pr merge` supports:

| Option     | Description                     |
| ---------- | ------------------------------- |
| `--merge`  | Regular merge commit            |
| `--squash` | Squash all commits into one     |
| `--rebase` | Rebase commits onto base branch |

These correspond to GitHub’s merge strategies.

---

## ❓ How Would You Review Someone Else’s PR Using `gh`?

### 🔹 1️⃣ Checkout the PR locally

```bash
gh pr checkout 2
```

---

### 🔹 2️⃣ Review changes

```bash
git diff main
```

---

### 🔹 3️⃣ Add a review (Approve / Comment / Request Changes)

Approve:

```bash
gh pr review 2 --approve
```

Request changes:

```bash
gh pr review 2 --request-changes --body "Please fix indentation."
```

Comment:

```bash
gh pr review 2 --comment --body "Looks good overall."
```

---

### 🔹 4️⃣ View checks & CI status

```bash
gh pr checks 2
```

---

# 🚀 Real DevOps Use Case

In CI/CD pipelines:

* Auto-create PRs
* Auto-merge if checks pass
* Block merge if tests fail
* Enforce review policies

---

# 🎯 Interview-Level Answer

> Using GitHub CLI, we can create, review, and merge pull requests entirely from the terminal. It supports merge, squash, and rebase strategies. It’s useful for automation, DevOps workflows, and scripting PR approvals in CI/CD pipelines.

---

---
### Task 5: GitHub Actions & Workflows (Preview)
1. List the workflow runs on any public repo that uses GitHub Actions
2. View the status of a specific workflow run
3. Answer in your notes: How could `gh run` and `gh workflow` be useful in a CI/CD pipeline?


   ---
   Here are clean **notes-style answers** for GitHub Actions (Preview) 👇

Using GitHub CLI (`gh`) with GitHub.

---

# ✅ GitHub Actions & Workflows (Preview)

We’ll use a public repo that uses GitHub Actions, for example:
Kubernetes

You can test on any repo that has Actions enabled.

---

# 1️⃣ List Workflow Runs on a Public Repo

```bash
gh run list --repo kubernetes/kubernetes
```

Output shows:

* Run ID
* Workflow name
* Branch
* Status (queued, in_progress, completed)
* Conclusion (success, failure, cancelled)

You can limit:

```bash
gh run list --repo kubernetes/kubernetes --limit 5
```

---

# 2️⃣ View Status of a Specific Workflow Run

First get Run ID from previous command.

Then:

```bash
gh run view <run-id> --repo kubernetes/kubernetes
```

Example:

```bash
gh run view 123456789 --repo kubernetes/kubernetes
```

To see logs:

```bash
gh run view <run-id> --log --repo kubernetes/kubernetes
```

To see only jobs:

```bash
gh run view <run-id> --json jobs --repo kubernetes/kubernetes
```

---

# 🔎 View Available Workflows

```bash
gh workflow list --repo kubernetes/kubernetes
```

View specific workflow:

```bash
gh workflow view CI --repo kubernetes/kubernetes
```

---

# 🧠 Notes Question

## ❓ How Could `gh run` and `gh workflow` Be Useful in CI/CD Pipeline?

---

## 🔹 1️⃣ Monitor Pipeline from Terminal

Instead of opening browser:

```bash
gh run list
gh run view <run-id>
```

Useful for:

* Production monitoring
* Debugging failed deployments
* DevOps on-call support

---

## 🔹 2️⃣ Trigger Workflow Manually

```bash
gh workflow run deploy.yml
```

Use case:

* Manual production deployment
* Hotfix release trigger
* Emergency rollback

---

## 🔹 3️⃣ Automate Checks in Scripts

Example in shell script:

```bash
if gh run list --limit 1 | grep -q failure; then
  echo "Last build failed"
fi
```

Used in:

* Automated release pipelines
* Deployment validation scripts

---

## 🔹 4️⃣ Download Logs Automatically

```bash
gh run download <run-id>
```

Helpful for:

* Incident investigation
* Automated log archiving
* Compliance storage

---

## 🔹 5️⃣ Auto-Retry Failed Workflows

```bash
gh run rerun <run-id>
```

Useful in:

* Flaky test environments
* Temporary infrastructure issues

---

# 🚀 Real DevOps Use Case

In enterprise CI/CD:

* Monitor pipeline health
* Trigger releases
* Automate rollback
* Fetch logs during outages
* Integrate with Slack/Teams alerts

---

# 🎯 Interview-Level Answer

> `gh run` and `gh workflow` allow DevOps engineers to monitor, trigger, and manage GitHub Actions workflows directly from the terminal. They can be integrated into scripts for automation, pipeline monitoring, incident handling, and deployment management without requiring the GitHub UI.

---
   ---
---


---

---

---
### Task 6: Useful `gh` Tricks
Explore and try these — add the ones you find useful to your `git-commands.md`:
1. `gh api` — make raw GitHub API calls from the terminal
2. `gh gist` — create and manage GitHub Gists
3. `gh release` — create and manage releases
4. `gh alias` — create shortcuts for commands you use often
5. `gh search repos` — search GitHub repos from the terminal

6. ---
7. [Git-commands (1).pdf](https://github.com/user-attachments/files/25516663/Git-commands.1.pdf)

8. ---
---

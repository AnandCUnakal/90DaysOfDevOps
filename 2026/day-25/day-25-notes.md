[Git-commands.pdf](https://github.com/user-attachments/files/25515123/Git-commands.pdf)### Task 1: Git Reset — Hands-On
1. Make 3 commits in your practice repo (commit A, B, C)
2. Use `git reset --soft` to go back one commit — what happens to the changes?
3. Re-commit, then use `git reset --mixed` to go back one commit — what happens now?
4. Re-commit, then use `git reset --hard` to go back one commit — what happens this time?
5. Answer in your notes:
   - What is the difference between `--soft`, `--mixed`, and `--hard`?
   - Which one is destructive and why?
   - When would you use each one?
   - Should you ever use `git reset` on commits that are already pushed?
---
Here are clean **notes-style answers** for your Git Reset hands-on 👇

---

# ✅ Task 1: Git Reset — Hands-On Notes

---

## 🔹 Step 1: Create 3 Commits

```bash
echo "A" >> file.txt
git commit -am "Commit A"

echo "B" >> file.txt
git commit -am "Commit B"

echo "C" >> file.txt
git commit -am "Commit C"
```

Now your history looks like:

```
A → B → C (HEAD)
```

---

# 🔹 Step 2: `git reset --soft HEAD~1`

```bash
git reset --soft HEAD~1
```

### 🔎 What happens?

* Moves HEAD from C → B
* Commit C is removed from history
* Changes from commit C remain **staged**
* Working directory unchanged

### 📌 Status after command:

```
Changes to be committed:
  modified: file.txt
```

👉 You can immediately recommit.

---

# 🔹 Step 3: Re-commit, then `git reset --mixed HEAD~1`

```bash
git commit -am "Commit C again"
git reset --mixed HEAD~1
```

### 🔎 What happens?

* Moves HEAD from C → B
* Commit C removed
* Changes from C remain in working directory
* But changes are **unstaged**

### 📌 Status after command:

```
Changes not staged for commit:
  modified: file.txt
```

👉 You must `git add` again before committing.

📌 NOTE: `--mixed` is default reset mode.

---

# 🔹 Step 4: Re-commit, then `git reset --hard HEAD~1`

```bash
git commit -am "Commit C again"
git reset --hard HEAD~1
```

### 🔎 What happens?

* Moves HEAD from C → B
* Commit C deleted
* Changes from C removed from staging
* Changes removed from working directory

💥 The changes are completely gone.

---

# 📊 Clear Comparison Table

| Reset Type | Commit History | Staging Area         | Working Directory |
| ---------- | -------------- | -------------------- | ----------------- |
| `--soft`   | Moves back     | Keeps staged changes | Keeps changes     |
| `--mixed`  | Moves back     | Unstages changes     | Keeps changes     |
| `--hard`   | Moves back     | Clears staging       | Deletes changes   |

---

# 🧠 Answers for Notes

---

## 1️⃣ Difference Between `--soft`, `--mixed`, `--hard`

### 🔹 `--soft`

* Only moves HEAD
* Keeps changes staged
* Safest reset

---

### 🔹 `--mixed` (default)

* Moves HEAD
* Unstages changes
* Keeps file changes

---

### 🔹 `--hard`

* Moves HEAD
* Deletes staging
* Deletes working directory changes

---

## 2️⃣ Which One is Destructive?

👉 `git reset --hard`

Because:

* It permanently deletes uncommitted changes
* Cannot recover unless using `git reflog`

---

## 3️⃣ When Would You Use Each?

### ✅ Use `--soft` when:

* You want to combine commits
* You want to edit last commit message
* You want to squash locally

---

### ✅ Use `--mixed` when:

* You committed too early
* You want to modify files before recommitting
* You want to unstage files

---

### ⚠ Use `--hard` when:

* You want to completely discard changes
* Cleaning local repo
* Resetting to match remote branch

Example:

```bash
git fetch
git reset --hard origin/main
```

---

## 4️⃣ Should You Use `git reset` on Pushed Commits?

🚨 Generally NO.

If commits are already pushed to shared remote (like in GitHub):

* Reset rewrites history
* Requires force push
* Can break other developers' work

Only use if:

* Working alone
* Or team agrees
* Or branch is not shared

Otherwise use:

```bash
git revert <commit>
```

Safer for shared repos.

---
---
---

---
### Task 2: Git Revert — Hands-On
1. Make 3 commits (commit X, Y, Z)
2. Revert commit Y (the middle one) — what happens?
3. Check `git log` — is commit Y still in the history?
4. Answer in your notes:
   - How is `git revert` different from `git reset`?
   - Why is revert considered **safer** than reset for shared branches?
   - When would you use revert vs reset?

     ---
     Here are clean **notes-style answers** for your Git Revert hands-on 👇

---

# ✅ Task 2: Git Revert — Hands-On Notes

---

## 🔹 Step 1: Create 3 Commits

```bash
echo "X" >> file.txt
git commit -am "Commit X"

echo "Y" >> file.txt
git commit -am "Commit Y"

echo "Z" >> file.txt
git commit -am "Commit Z"
```

History:

```
X → Y → Z (HEAD)
```

---

# 🔹 Step 2: Revert Commit Y (Middle Commit)

First, find commit hash:

```bash
git log --oneline
```

Assume:

```
a1b2c3 Commit Z
d4e5f6 Commit Y
g7h8i9 Commit X
```

Now revert Y:

```bash
git revert d4e5f6
```

---

## 🔎 What Happens?

* Git creates a **new commit**
* That new commit **undoes the changes introduced by Y**
* History is preserved
* No commits are deleted

New history:

```
X → Y → Z → R (revert Y)
```

Where **R** is a new commit like:

```
Revert "Commit Y"
```

---

# 🔹 Step 3: Is Commit Y Still in History?

✅ YES.

Run:

```bash
git log --oneline
```

You will see:

* Commit Y still exists
* A new revert commit is added

👉 Revert does NOT remove history.

---

# 🧠 Answers for Notes

---

## 1️⃣ How is `git revert` different from `git reset`?

| Feature                   | `git revert` | `git reset` |
| ------------------------- | ------------ | ----------- |
| Deletes commits?          | ❌ No         | ✅ Yes       |
| Creates new commit?       | ✅ Yes        | ❌ No        |
| Safe for shared branches? | ✅ Yes        | ❌ Risky     |
| Rewrites history?         | ❌ No         | ✅ Yes       |

---

### 🔹 Simple Explanation

* `reset` moves branch pointer backward (history rewrite)
* `revert` creates a new commit that undoes changes

---

## 2️⃣ Why is Revert Safer for Shared Branches?

Because:

* It does NOT rewrite history
* It does NOT require force push
* Other developers' history stays intact
* No risk of breaking team workflow

In platforms like GitHub:

* Reset + force push can overwrite others' commits
* Revert is the recommended approach for production branches

---

## 3️⃣ When Would You Use Revert vs Reset?

---

### ✅ Use `git revert` when:

* Commit is already pushed
* Working on shared branch (main, develop, release)
* Fixing production issue
* Team collaboration

Example:

```bash
git revert <commit-hash>
git push
```

---

### ⚠ Use `git reset` when:

* Commit is local only
* Cleaning up history before pushing
* Squashing commits locally
* Fixing recent mistake

Example:

```bash
git reset --soft HEAD~1
```

---

# 🔥 Real-World DevOps Scenario

### 🚨 Production Bug Found

Bad commit already merged to `main`.

Correct approach:

```bash
git revert <bad-commit>
```

Push fix → CI/CD runs → safe rollback.

Never:

```bash
git reset --hard
git push --force
```

---
     ---
---


---

### Task 3: Reset vs Revert — Summary
Create a comparison in your notes:

| | `git reset` | `git revert` |
|---|---|---|
| What it does | ? | ? |
| Removes commit from history? | ? | ? |
| Safe for shared/pushed branches? | ? | ? |
| When to use | ? | ? |

|                                      | `git reset`                                                             | `git revert`                                                        |
| ------------------------------------ | ----------------------------------------------------------------------- | ------------------------------------------------------------------- |
| **What it does**                     | Moves the branch pointer (HEAD) to a previous commit                    | Creates a new commit that undoes changes of a previous commit       |
| **Removes commit from history?**     | ✅ Yes (rewrites history)                                                | ❌ No (history remains intact)                                       |
| **Safe for shared/pushed branches?** | ❌ No (can break team history if force pushed)                           | ✅ Yes (does not rewrite history)                                    |
| **When to use**                      | Local commits, cleaning up history, undoing recent mistakes before push | Undoing changes on shared branches, production fixes, safe rollback |

🔎 Quick Explanation
🔹 git reset

        Changes commit history
        Can modify staging + working directory depending on --soft, --mixed, --hard
        Dangerous if already pushed to remote (like on GitHub)

🔹 git revert
          Keeps history intact
          Adds a new commit that reverses changes
          Best for team environments and CI/CD workflows
---



---

### Task 4: Branching Strategies
Research the following branching strategies and document each in your notes with:
- How it works (short description)
- A simple diagram or flow (text-based is fine)
- When/where it's used
- Pros and cons

1. **GitFlow** — develop, feature, release, hotfix branches
2. **GitHub Flow** — simple, single main branch + feature branches
3. **Trunk-Based Development** — everyone commits to main, short-lived branches
4. Answer:
   - Which strategy would you use for a startup shipping fast?
   - Which strategy would you use for a large team with scheduled releases?
   - Which one does your favorite open-source project use? (check any repo on GitHub)
---
Here are structured **notes-style documentation answers** for Task 4 👇

---

# ✅ Task 4: Branching Strategies

---

# 1️⃣ GitFlow

Originally described by Vincent Driessen.

---

## 🔹 How It Works (Short Description)

GitFlow uses multiple long-lived branches:

* `main` → production-ready code
* `develop` → integration branch
* `feature/*` → new features (branched from develop)
* `release/*` → release preparation
* `hotfix/*` → urgent production fixes (branched from main)

Development happens in `feature` → merged into `develop` → release branch → merged into `main`.

---

## 🔹 Simple Diagram

```text
main      ●──────────────●──────────────●
           \              \            /
hotfix      ●              ●───────────
             \
develop   ●────●────●────●────●────●────
            \        \        \
feature      ●        ●        ●
                     \
release               ●────●
```

---

## 🔹 When / Where It's Used

* Enterprises
* Banking / insurance systems
* Large teams
* Scheduled release cycles (e.g., monthly/quarterly)
* Strict QA processes

---

## 🔹 Pros

✅ Structured workflow
✅ Clear separation of environments
✅ Supports parallel development
✅ Strong release management

---

## 🔹 Cons

❌ Complex for small teams
❌ Heavy merging
❌ Slower release velocity
❌ Overhead for CI/CD-heavy startups

---

# 2️⃣ GitHub Flow

Popularized by GitHub.

---

## 🔹 How It Works (Short Description)

* Only one long-lived branch → `main`
* Create short-lived feature branch from `main`
* Open Pull Request (PR)
* Code review
* Merge to `main`
* Deploy

No `develop`, no `release` branches.

---

## 🔹 Simple Diagram

```text
main  ●──────●──────●──────●──────
         \       \       \
feature   ●       ●       ●
```

---

## 🔹 When / Where It's Used

* Startups
* SaaS companies
* Continuous deployment environments
* Web applications
* Open-source projects

Example: React uses PR-based workflow similar to GitHub Flow.

---

## 🔹 Pros

✅ Simple
✅ Easy onboarding
✅ Works well with CI/CD
✅ Fast releases
✅ Encourages small PRs

---

## 🔹 Cons

❌ Requires strong automated testing
❌ Less formal release structure
❌ Can become messy without discipline

---

# 3️⃣ Trunk-Based Development (TBD)

Widely used by high-scale engineering teams like Google.

---

## 🔹 How It Works (Short Description)

* Single main branch (`main` or `trunk`)
* Developers commit directly to trunk
* Feature branches are very short-lived (hours or 1–2 days)
* Heavy automation + CI required
* Feature flags used for incomplete features

---

## 🔹 Simple Diagram

```text
main  ●─●─●─●─●─●─●─●─●─●
        |  |  |  |  |
       small short-lived branches
```

---

## 🔹 When / Where It's Used

* DevOps-mature teams
* Microservices architecture
* Continuous integration environments
* Large tech companies
* High deployment frequency systems

---

## 🔹 Pros

✅ Very fast integration
✅ Fewer merge conflicts
✅ Encourages small commits
✅ Perfect for CI/CD pipelines

---

## 🔹 Cons

❌ Requires excellent test coverage
❌ Risky without automation
❌ Demands disciplined developers
❌ Hard for low-maturity teams

---

# 📊 Quick Comparison

| Strategy    | Structure                          | Speed       | Best For            |
| ----------- | ---------------------------------- | ----------- | ------------------- |
| GitFlow     | High                               | Medium/Slow | Large enterprises   |
| GitHub Flow | Medium                             | Fast        | Startups & SaaS     |
| Trunk-Based | Minimal structure, high discipline | Very Fast   | DevOps mature teams |

---

# 🎯 Final Answers

---

## 🔹 Which strategy for a startup shipping fast?

👉 **GitHub Flow**

Reason:

* Simple
* Fast releases
* Works perfectly with CI/CD
* Low overhead

Alternative if highly automated team → Trunk-Based Development.

---

## 🔹 Which strategy for a large team with scheduled releases?

👉 **GitFlow**

Reason:

* Clear release management
* Dedicated hotfix support
* Works well with structured QA
* Better control over versioning

---

## 🔹 Which one does your favorite open-source project use?

Most modern open-source projects on GitHub use **GitHub Flow**.

Example:

* Kubernetes uses PR-based workflow with release branches (hybrid GitHub Flow + release model).

---

# 🎯 Strong Interview Summary (5–8 Years Experience)

> GitFlow is structured and suitable for enterprises with scheduled releases. GitHub Flow is lightweight and ideal for startups and continuous deployment. Trunk-Based Development is the fastest approach but requires strong automation and CI maturity.

---
---
---

---
---

### Task 5: Git Commands Reference Update
Update your `git-commands.md` to cover everything from Days 22–25:
- Setup & Config
- Basic Workflow (add, commit, status, log, diff)
- Branching (branch, checkout, switch)
- Remote (push, pull, fetch, clone, fork)
- Merging & Rebasing
- Stash & Cherry Pick
- Reset & Revert

---
Git Commands Reference (Days 22–25)
1️⃣ Setup & Config
Check Git Version
git --version
Configure Username & Email
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
View Config
git config --list
Initialize Repository
git init

2️⃣ Basic Workflow
Check Status
git status
Add Files
git add file.txt
git add .
Commit Changes
git commit -m "Commit message"
View Commit History
git log
git log --oneline
git log --graph --oneline --all
View Differences
git diff                # unstaged changes
git diff --staged       # staged changes

3️⃣ Branching
Create Branch
git branch feature-name
Switch Branch
git checkout feature-name
# or
git switch feature-name
Create & Switch
git checkout -b feature-name
# or
git switch -c feature-name
Delete Branch
git branch -d feature-name

4️⃣ Remote Operations
Add Remote
git remote add origin <repo-url>
View Remotes
git remote -v
Push Changes
git push origin main
Pull Changes
git pull origin main
Fetch Changes
git fetch origin
Clone Repository
git clone <repo-url>
Fork (Concept)

Forking is done via Git hosting platform UI, then clone your fork locally.

5️⃣ Merging & Rebasing
Merge Branch
git merge feature-branch
Squash Merge
git merge --squash feature-branch
Rebase
git rebase main
Interactive Rebase
git rebase -i HEAD~3

6️⃣ Stash & Cherry Pick
Stash Changes
git stash
git stash -u              # include untracked
List Stashes
git stash list
Apply Stash
git stash apply stash@{0}
Pop Stash
git stash pop
Drop Stash
git stash drop stash@{0}
Cherry Pick Commit
git cherry-pick <commit-hash>

7️⃣ Reset & Revert
Reset (Move HEAD)
Soft Reset
git reset --soft HEAD~1
Mixed Reset (Default)
git reset --mixed HEAD~1
Hard Reset
git reset --hard HEAD~1
Reset to Remote
git fetch origin
git reset --hard origin/main
Revert Commit
git revert <commit-hash>

🔥 Quick Comparison
Command	Use Case
reset --soft	Undo commit, keep staged
reset --mixed	Undo commit, keep changes unstaged
reset --hard	Delete commit and changes
revert	Safely undo commit with new commit
stash	Temporarily save uncommitted work
cherry-pick	Apply specific commit to current branch
rebase	Reapply commits on new base
merge	Combine branch histories

📌 Best Practices
Avoid reset --hard on shared branches.
Use revert for production or pushed commits.
Keep feature branches short-lived.
Use meaningful commit messages.
Always pull before pushing in team environments.
---


[Git-commands.pdf](https://github.com/user-attachments/files/25515126/Git-commands.pdf)


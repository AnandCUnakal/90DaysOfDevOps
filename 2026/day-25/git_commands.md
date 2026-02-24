# Git Commands Reference (Days 22–25)

---

# 1️⃣ Setup & Config

## Check Git Version
```bash
git --version
```

## Configure Username & Email
```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

## View Config
```bash
git config --list
```

## Initialize Repository
```bash
git init
```

---

# 2️⃣ Basic Workflow

## Check Status
```bash
git status
```

## Add Files
```bash
git add file.txt
git add .
```

## Commit Changes
```bash
git commit -m "Commit message"
```

## View Commit History
```bash
git log
git log --oneline
git log --graph --oneline --all
```

## View Differences
```bash
git diff                # unstaged changes
git diff --staged       # staged changes
```

---

# 3️⃣ Branching

## Create Branch
```bash
git branch feature-name
```

## Switch Branch
```bash
git checkout feature-name
# or
git switch feature-name
```

## Create & Switch
```bash
git checkout -b feature-name
# or
git switch -c feature-name
```

## Delete Branch
```bash
git branch -d feature-name
```

---

# 4️⃣ Remote Operations

## Add Remote
```bash
git remote add origin <repo-url>
```

## View Remotes
```bash
git remote -v
```

## Push Changes
```bash
git push origin main
```

## Pull Changes
```bash
git pull origin main
```

## Fetch Changes
```bash
git fetch origin
```

## Clone Repository
```bash
git clone <repo-url>
```

## Fork (Concept)
Forking is done via Git hosting platform UI, then clone your fork locally.

---

# 5️⃣ Merging & Rebasing

## Merge Branch
```bash
git merge feature-branch
```

## Squash Merge
```bash
git merge --squash feature-branch
```

## Rebase
```bash
git rebase main
```

## Interactive Rebase
```bash
git rebase -i HEAD~3
```

---

# 6️⃣ Stash & Cherry Pick

## Stash Changes
```bash
git stash
git stash -u              # include untracked
```

## List Stashes
```bash
git stash list
```

## Apply Stash
```bash
git stash apply stash@{0}
```

## Pop Stash
```bash
git stash pop
```

## Drop Stash
```bash
git stash drop stash@{0}
```

## Cherry Pick Commit
```bash
git cherry-pick <commit-hash>
```

---

# 7️⃣ Reset & Revert

## Reset (Move HEAD)

### Soft Reset
```bash
git reset --soft HEAD~1
```

### Mixed Reset (Default)
```bash
git reset --mixed HEAD~1
```

### Hard Reset
```bash
git reset --hard HEAD~1
```

## Reset to Remote
```bash
git fetch origin
git reset --hard origin/main
```

## Revert Commit
```bash
git revert <commit-hash>
```

---

# 🔥 Quick Comparison

| Command | Use Case |
|----------|----------|
| reset --soft | Undo commit, keep staged |
| reset --mixed | Undo commit, keep changes unstaged |
| reset --hard | Delete commit and changes |
| revert | Safely undo commit with new commit |
| stash | Temporarily save uncommitted work |
| cherry-pick | Apply specific commit to current branch |
| rebase | Reapply commits on new base |
| merge | Combine branch histories |

---

# 📌 Best Practices

- Avoid `reset --hard` on shared branches.
- Use `revert` for production or pushed commits.
- Keep feature branches short-lived.
- Use meaningful commit messages.
- Always pull before pushing in team environments.

---

End of Git Commands Reference (Days 22–25)


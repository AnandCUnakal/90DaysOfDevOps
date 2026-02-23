### Task 1: Git Merge — Hands-On
1. Create a new branch `feature-login` from `main`, add a couple of commits to it
2. Switch back to `main` and merge `feature-login` into `main`
3. Observe the merge — did Git do a **fast-forward** merge or a **merge commit**?
4. Now create another branch `feature-signup`, add commits to it — but also add a commit to `main` before merging
5. Merge `feature-signup` into `main` — what happens this time?
6. Answer in your notes:
   - What is a fast-forward merge?
   - When does Git create a merge commit instead?
   - What is a merge conflict? (try creating one intentionally by editing the same line in both branches)
  
     <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/aa4dc80d-d7fd-4de2-8850-afd35b1ba419" />

      anand@DESKTOP-H50FBNH MINGW64 /d/ANDYtws/git_practice/24day/90DaysOfDevOps/2026/day-24 (main)
      $ git branch feature-signup
      
      anand@DESKTOP-H50FBNH MINGW64 /d/ANDYtws/git_practice/24day/90DaysOfDevOps/2026/day-24 (main)
      $ git status
      On branch main
      Untracked files:
        (use "git add <file>..." to include in what will be committed)
              check
              signupfile
      
      nothing added to commit but untracked files present (use "git add" to track)

     <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f51554b8-0048-4555-9c8a-9b59e94cf202" />


---

### Task 2: Git Rebase — Hands-On
1. Create a branch `feature-dashboard` from `main`, add 2-3 commits
2. While on `main`, add a new commit (so `main` moves ahead)
3. Switch to `feature-dashboard` and rebase it onto `main`
4. Observe your `git log --oneline --graph --all` — how does the history look compared to a merge?
5. Answer in your notes:
   - What does rebase actually do to your commits?
   - How is the history different from a merge?
   - Why should you **never rebase commits that have been pushed and shared** with others?
   - When would you use rebase vs merge?

  <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e77a18e2-c1e1-4e8f-a96d-2de4ecc4307c" />

---

---
### Task 3: Squash Commit vs Merge Commit
1. Create a branch `feature-profile`, add 4-5 small commits (typo fix, formatting, etc.)
2. Merge it into `main` using `--squash` — what happens?
3. Check `git log` — how many commits were added to `main`?
4. Now create another branch `feature-settings`, add a few commits
5. Merge it into `main` **without** `--squash` (regular merge) — compare the history
6. Answer in your notes:
   - What does squash merging do?
   - Squash merging:
        Combines all commits from a feature branch into one single commit
        Removes detailed commit history
        Keeps main branch history clean and linear
        Does NOT create a merge commit
     
   - When would you use squash merge vs regular merge?
   -     ✅ Use Squash Merge When:
              Feature branch has many small/fix/typo commits
              You want clean commit history
              Working in small teams
              You want 1 commit per feature
              In code review workflows (like in GitHub pull requests)
         ✅ Use Regular Merge When:
              You want full commit history preserved
              Working on large or complex features
              Need traceability for debugging
              Multiple developers worked on the branch
              Open-source projects


   - What is the trade-off of squashing?
   - ✔ Advantages
            Clean commit history
            Easier to read git log
            Avoids clutter         
            Better for production branch
      ❌ Disadvantages
            Lose detailed commit history
            Harder to track which small commit introduced a bug
            Harder to revert specific internal changes
  
       🔥 Visual Comparison
            🟢 Squash Merge
            main
              |
              A
              B
              C
              D  (feature commits)
              ↓
              E  (ONE squashed commit)
            🔵 Regular Merge
            main
              |
              A
               \
                B
                C
                D
               /
              M  (merge commit)

     <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4d428814-e0f7-4e67-a64e-bdbfd41213e0" />

---

---

### Task 4: Git Stash — Hands-On
1. Start making changes to a file but **do not commit**
2. Now imagine you need to urgently switch to another branch — try switching. What happens?
3. Use `git stash` to save your work-in-progress
4. Switch to another branch, do some work, switch back
5. Apply your stashed changes using `git stash pop`

   <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/43b4cf08-ea11-4d10-b619-67b01c076f03" />

7. Try stashing multiple times and list all stashes
8. Try applying a specific stash from the list
9. Answer in your notes:
   - What is the difference between `git stash pop` and `git stash apply`?
   - When would you use stash in a real-world workflow?


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b76e91fc-541e-4028-a1c9-730ec8fa39c6" />

---


---

### Task 5: Cherry Picking
1. Create a branch `feature-hotfix`, make 3 commits with different changes
2. Switch to `main`
3. Cherry-pick **only the second commit** from `feature-hotfix` onto `main`
4. Verify with `git log` that only that one commit was applied
5. Answer in your notes:
   - What does cherry-pick do?
   - When would you use cherry-pick in a real project?
   - What can go wrong with cherry-picking?


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/96779486-c593-4579-af94-795862e4cd9b" />

---


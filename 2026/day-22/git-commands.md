### Task 1: Install and Configure Git
1. Verify Git is installed on your machine
2. Set up your Git identity — name and email
3. Verify your configuration


'''
Perfect 👍 — let’s go step-by-step to install and configure **Git** properly.

---

# ✅ Task 1: Install and Configure Git

---
            
            # 1️⃣ Verify Git is Installed
            
            Open terminal and run:
            
            ```bash
            git --version
            ```
            
            ### ✅ If Installed
            
            You’ll see something like:
            
            ```
            git version 2.43.0
            ```
            
            ---
            
            ### ❌ If Not Installed
            
            ### 🔹 On Ubuntu/Debian
            
            ```bash
            sudo apt update
            sudo apt install git -y
            ```
            
            ---
            
            ### 🔹 On RHEL/CentOS
            
            ```bash
            sudo yum install git -y
            ```
            
            ---
            
            ### 🔹 On macOS (Homebrew)
            
            ```bash
            brew install git
            ```
            
            ---
            
            ### 🔹 On Windows
            
            Install from:
            👉 [https://git-scm.com/download/win](https://git-scm.com/download/win)
            Then reopen terminal and check:
            
            ```bash
            git --version
            ```
            
            ---
            
            # 2️⃣ Set Up Your Git Identity
            
            Git needs your name and email for commits.
            
            ---
            
            ## 🔹 Set Globally (Recommended)
            
            ```bash
            git config --global user.name "Your Name"
            git config --global user.email "your@email.com"
            ```
            
            Example:
            
            ```bash
            git config --global user.name "DevOps Engineer"
            git config --global user.email "devops@example.com"
            ```
            
            ---
            
            ## 🔹 Set for Specific Repository Only
            
            Inside project folder:
            
            ```bash
            git config user.name "Project Name"
            git config user.email "project@email.com"
            ```
            
            ---
            
            # 3️⃣ Verify Configuration
            
            ---
            
            ## 🔹 Check Global Settings
            
            ```bash
            git config --global --list
            ```
            
            You should see:
            
            ```
            user.name=DevOps Engineer
            user.email=devops@example.com
            ```
            
            ---
            
            ## 🔹 Check Specific Value
            
            ```bash
            git config user.name
            git config user.email
            ```
            
            ---
            
            # 🔥 Optional (Highly Recommended for DevOps)
            
            ## Set Default Branch to `main`
            
            ```bash
            git config --global init.defaultBranch main
            ```
            
            ---
            
            ## Enable Helpful Settings
            
            ```bash
            git config --global color.ui auto
            git config --global core.editor nano
            ```
            
            (Or `vim` if you prefer)
            
            ---
            
            # 🎯 Quick Verification Checklist
            
            ✔ `git --version` works
            ✔ `git config --global --list` shows name & email
            ✔ Default branch is set to `main`
            
            ---
            
            '''
            
            '''
            Excellent 👌 — now we move from configuration to **hands-on Git repository creation**.
            
            Follow step-by-step and understand what Git is doing internally.
            
            ---
            
  # ✅ Task 2: Create Your Git Project
            
            ---
            
            # 1️⃣ Create a New Folder
            
            ```bash
            mkdir devops-git-practice
            cd devops-git-practice
            ```
            
            Verify:
            
            ```bash
            pwd
            ```
            
            You should be inside:
            
            ```
            .../devops-git-practice
            ```
            
            ---
            
            # 2️⃣ Initialize as a Git Repository
            
            ```bash
            git init
            ```
            
            ### ✅ Output Example:
            
            ```
            Initialized empty Git repository in /path/devops-git-practice/.git/
            ```
            
            Now your directory is officially a Git repository.
            
            ---
            
            # 3️⃣ Check Git Status
            
            ```bash
            git status
            ```
            
            ### 🔎 You’ll See Something Like:
            
            ```
            On branch main
            
            No commits yet
            
            nothing to commit (create/copy files and use "git add" to track)
            ```
            
            ---
            
            ## 🔍 What This Means
            
            | Message           | Meaning                |
            | ----------------- | ---------------------- |
            | On branch main    | Default branch created |
            | No commits yet    | Repository is empty    |
            | Nothing to commit | No files added yet     |
            
            Git is telling you:
            
            👉 Repo exists
            👉 No tracked files
            👉 No commit history
            
            ---
            
            # 4️⃣ Explore the Hidden `.git/` Directory
            
            This is where Git stores everything.
            
            ---
            
            ## 🔹 Show Hidden Files
            
            ```bash
            ls -la
            ```
            
            You’ll see:
            
            ```
            .git/
            ```
            
            ---
            
            ## 🔹 Explore Inside `.git`
            
            ```bash
            cd .git
            ls -la
            ```
            
            You’ll see something like:
            
            ```
            HEAD
            config
            description
            hooks/
            info/
            objects/
            refs/
            ```
            
            ---
            
            # 🧠 What These Mean
            
            | Folder/File | Purpose                  |
            | ----------- | ------------------------ |
            | `HEAD`      | Points to current branch |
            | `config`    | Repo-specific config     |
            | `objects/`  | Stores commits & data    |
            | `refs/`     | Branch references        |
            | `hooks/`    | Git hook scripts         |
            | `info/`     | Exclude rules            |
            
            ---
            
            ## 🔹 Check Current Branch Reference
            
            ```bash
            cat HEAD
            ```
            
            Output:
            
            ```
            ref: refs/heads/main
            ```
            
            This means:
            👉 HEAD points to `main` branch
            
            ---
            
            # 🚀 What You Just Learned
            
            When you ran:
            
            ```bash
            git init
            ```
            
            Git:
            
            * Created `.git/`
            * Set default branch
            * Created internal storage system
            * Initialized commit history structure
            
            ---
            
            # 🔥 DevOps Insight
            
            In CI/CD pipelines:
            
            * `.git/` contains commit metadata
            * Used for version tagging
            * Used for branch detection
            * Used for build versioning
            
            ---
            
            # 🎯 Next Recommended Step
            
            Now create your first file:
            
            ```bash
            echo "My first Git project" > README.md
            git status
            ```
            
            You’ll see:
            
            ```
            Untracked files:
              README.md
            ```
            
            Now Git sees a file but isn’t tracking it yet.

---

'''

'''
### Task 3: Create Your Git Commands Reference
1. Create a file called `git-commands.md` inside the repo
2. Add the Git commands you've used so far, organized by category:
   - **Setup & Config**
   - **Basic Workflow**
   - **Viewing Changes**
3. For each command, write:
   - What it does (1 line)
   - An example of how to use it
'''
'''
            Perfect 👌 — now we’ll create a **clean Git reference file** inside your project.
            
            This will help you:
            
            * Build habit of documentation
            * Understand commands deeply
            * Create a reusable DevOps reference
            
            ---
            
            # ✅ Step 1: Create the File
            
            Inside your `devops-git-practice` directory:
            
            ```bash
            touch git-commands.md
            ```
            
            Open it:
            
            ```bash
            nano git-commands.md
            ```
            
            (or `vim` / VS Code)
            
            ---
            
            # ✅ Step 2: Add This Content to `git-commands.md`
            
            Copy and paste the following:
            
            ---
            
            ````markdown
            # Git Commands Reference
            
            This document contains commonly used Git commands organized by category.
            
            ---
            
            # 🔹 Setup & Config
            
            ## 1. git --version
            **What it does:** Shows the installed Git version.  
            **Example:**
            ```bash
            git --version
            ````
            
            ## 2. git config --global user.name
            
            **What it does:** Sets your global Git username.
            **Example:**
            
            ```bash
            git config --global user.name "DevOps Engineer"
            ```
            
            ## 3. git config --global user.email
            
            **What it does:** Sets your global Git email.
            **Example:**
            
            ```bash
            git config --global user.email "devops@example.com"
            ```
            
            ## 4. git config --global --list
            
            **What it does:** Displays all global Git configurations.
            **Example:**
            
            ```bash
            git config --global --list
            ```
            
            ## 5. git init
            
            **What it does:** Initializes a new Git repository.
            **Example:**
            
            ```bash
            git init
            ```
            
            ---
            
            # 🔹 Basic Workflow
            
            ## 6. git status
            
            **What it does:** Shows the current status of working directory and staging area.
            **Example:**
            
            ```bash
            git status
            ```
            
            ## 7. git add
            
            **What it does:** Adds files to the staging area.
            **Example:**
            
            ```bash
            git add README.md
            ```
            
            ## 8. git add .
            
            **What it does:** Adds all modified/new files to staging area.
            **Example:**
            
            ```bash
            git add .
            ```
            
            ## 9. git commit -m
            
            **What it does:** Saves staged changes with a commit message.
            **Example:**
            
            ```bash
            git commit -m "Initial commit"
            ```
            
            ---
            
            # 🔹 Viewing Changes
            
            ## 10. git log
            
            **What it does:** Shows commit history.
            **Example:**
            
            ```bash
            git log
            ```
            
            ## 11. git log --oneline
            
            **What it does:** Shows compact commit history.
            **Example:**
            
            ```bash
            git log --oneline
            ```
            
            ## 12. git diff
            
            **What it does:** Shows changes not yet staged.
            **Example:**
            
            ```bash
            git diff
            ```
            
            ## 13. git diff --staged
            
            **What it does:** Shows staged changes before commit.
            **Example:**
            
            ```bash
            git diff --staged
            ```
            
            ---
            
            ```
            
            ---
            
            # ✅ Step 3: Save and Exit
            
            If using nano:
            
            ```
            
            CTRL + X → Y → Enter
            
            ````
            
            ---
            
            # ✅ Step 4: Commit This File
            
            ```bash
            git add git-commands.md
            git commit -m "Add Git commands reference"
            ````
            
            ---
            
            # 🎯 What You Just Did
            
            ✔ Created documentation inside repo
            ✔ Practiced real Git workflow
            ✔ Created reusable Git cheat sheet
            ✔ Made your first meaningful commit
            
            ---


'''

### Task 4: Stage and Commit
1. Stage your file
2. Check what's staged
3. Commit with a meaningful message
4. View your commit history

'''
              Excellent 🔥 — now you’re practicing the **core Git workflow** (this is 80% of daily DevOps Git usage).
              
              Assuming you're inside:
              
              ```
              devops-git-practice
              ```
              
              ---
              
              # ✅ 1️⃣ Stage Your File
              
              If you created `git-commands.md`, stage it:
              
              ```bash
              git add git-commands.md
              ```
              
              Or stage everything:
              
              ```bash
              git add .
              ```
              
              👉 This moves the file from **Working Directory → Staging Area**
              
              ---
              
              # ✅ 2️⃣ Check What’s Staged
              
              ```bash
              git status
              ```
              
              Look for:
              
              ```
              Changes to be committed:
                new file:   git-commands.md
              ```
              
              That means it is staged ✅
              
              You can also see exactly what is staged:
              
              ```bash
              git diff --staged
              ```
              
              ---
              
              # ✅ 3️⃣ Commit with a Meaningful Message
              
              ```bash
              git commit -m "Add initial Git commands reference documentation"
              ```
              
              Good commit messages:
              
              * Start with a verb
              * Be clear
              * Be specific
              
              ❌ Bad: `update`
              ✅ Good: `Add Git commands reference documentation`
              
              ---
              
              # ✅ 4️⃣ View Your Commit History
              
              Compact view:
              
              ```bash
              git log --oneline
              ```
              
              Detailed view:
              
              ```bash
              git log
              ```
              
              Example output:
              
              ```
              a1b2c3d Add initial Git commands reference documentation
              ```
              
              Each commit has:
              
              * Commit ID (SHA)
              * Author
              * Date
              * Message
              
              ---
              
              # 🧠 What Just Happened Internally
              
              ```
              Working Directory  →  Staging Area  →  Local Repository
                    (edit)             (add)            (commit)
              ```
              
              You created a snapshot of your project at that moment.
              
              ---
              
              # 🎯 Mini Challenge (Very Important)
              
              Now:
              
              1. Edit `git-commands.md`
              2. Add one more command (like `git branch`)
              3. Run:
              
                 ```bash
                 git status
                 git diff
                 git add .
                 git commit -m "Add git branch command"
                 git log --oneline
                 ```
              
              This repetition builds muscle memory 💪
              
              ---
              
              Ready for **branching & merging** next? That’s where real Git power begins 🚀

'''

### Task 5: Make More Changes and Build History
1. Edit `git-commands.md` — add more commands as you discover them
2. Check what changed since your last commit
3. Stage and commit again with a different, descriptive message
4. Repeat this process at least **3 times** so you have multiple commits in your history
5. View the full history in a compact format

   '''
---

          # 🔁 Commit Cycle 1
          
          ## 1️⃣ Edit the file
          
          Open:
          
          ```bash
          nano git-commands.md
          ```
          
          Add a new section:
          
          ````markdown
          ## 14. git branch
          **What it does:** Lists branches in the repository.
          **Example:**
          ```bash
          git branch
          ````
          
          ````
          
          Save and exit.
          
          ---
          
          ## 2️⃣ Check what changed
          
          ```bash
          git status
          ````
          
          Then:
          
          ```bash
          git diff
          ```
          
          This shows what changed since last commit.
          
          ---
          
          ## 3️⃣ Stage and Commit
          
          ```bash
          git add git-commands.md
          git commit -m "Add git branch command explanation"
          ```
          
          ---
          
          # 🔁 Commit Cycle 2
          
          ## 1️⃣ Add another command
          
          Add:
          
          ````markdown
          ## 15. git checkout
          **What it does:** Switches branches.
          **Example:**
          ```bash
          git checkout main
          ````
          
          ````
          
          ---
          
          ## 2️⃣ Check changes
          
          ```bash
          git diff
          ````
          
          ---
          
          ## 3️⃣ Commit
          
          ```bash
          git add .
          git commit -m "Add git checkout command details"
          ```
          
          ---
          
          # 🔁 Commit Cycle 3
          
          Add:
          
          ````markdown
          ## 16. git merge
          **What it does:** Merges another branch into current branch.
          **Example:**
          ```bash
          git merge feature-branch
          ````
          
          ````
          
          ---
          
          Then:
          
          ```bash
          git status
          git diff
          git add .
          git commit -m "Document git merge command"
          ````
          
          ---
          
          # ✅ 4️⃣ View Full History (Compact Format)
          
          ```bash
          git log --oneline
          ```
          
          Example output:
          
          ```
          e4f5g6h Document git merge command
          d3e4f5g Add git checkout command details
          c2d3e4f Add git branch command explanation
          b1c2d3e Add initial Git commands reference documentation
          ```
          
          Now you have real history 👌
          
          ---
          
          # 🔥 Pro Tip (Very Important)
          
          Run:
          
          ```bash
          git log --oneline --graph --decorate
          ```
          
          This shows:
          
          * Commit history
          * Branch pointers
          * Graph structure
          
          This is how DevOps engineers visualize history.
          
          ---
          
          # 🧠 What You Just Learned
          
          ✔ Every commit = snapshot
          ✔ History builds incrementally
          ✔ Small commits = clean history
          ✔ Git is timeline-based
          
          ---
          
          # 🎯 Next Level Challenge
          
          Want to:
          
          * Create a new branch?
          * Modify file differently in two branches?
          * Merge them?
          * Intentionally create a merge conflict?
          
          That’s where Git mastery starts 🚀

   '''
### Task 6: Understand the Git Workflow
Answer these questions in your own words (add them to a `day-22-notes.md` file):
1. What is the difference between `git add` and `git commit`?
2. What does the **staging area** do? Why doesn't Git just commit directly?
3. What information does `git log` show you?
4. What is the `.git/` folder and what happens if you delete it?
5. What is the difference between a **working directory**, **staging area**, and **repository**?


'''
                  Excellent 👏 — this is where you move from “using Git” to **understanding Git**.
                  
                  Let’s create your notes file properly.
                  
                  ---
                  
                  # ✅ Step 1: Create the File
                  
                  ```bash
                  touch day-22-notes.md
                  ```
                  
                  Open it:
                  
                  ```bash
                  nano day-22-notes.md
                  ```
                  
                  ---
                  
                  # ✅ Step 2: Add This Content (You Can Modify in Your Own Words)
                  
                  ```markdown
                  # Day 22 – Understanding Git Workflow
                  
                  ---
                  
                  ## 1. What is the difference between `git add` and `git commit`?
                  
                  `git add` moves changes from the working directory to the staging area.  
                  `git commit` takes the staged changes and saves them permanently in the local repository as a snapshot.
                  
                  In short:
                  - git add = prepare changes
                  - git commit = save changes
                  
                  ---
                  
                  ## 2. What does the staging area do? Why doesn't Git just commit directly?
                  
                  The staging area allows us to choose exactly which changes we want to include in the next commit.
                  
                  It acts as a preparation area before committing.  
                  Git does not commit directly because sometimes we modify multiple files but only want to commit specific changes.
                  
                  The staging area gives better control and cleaner history.
                  
                  ---
                  
                  ## 3. What information does `git log` show you?
                  
                  `git log` shows:
                  
                  - Commit ID (SHA)
                  - Author name
                  - Author email
                  - Date of commit
                  - Commit message
                  
                  It helps track the history of changes in the repository.
                  
                  ---
                  
                  ## 4. What is the `.git/` folder and what happens if you delete it?
                  
                  The `.git/` folder is the hidden directory that stores all Git metadata, including:
                  
                  - Commit history
                  - Branch information
                  - Configuration
                  - Object database
                  
                  If you delete the `.git/` folder:
                  - The project will no longer be a Git repository.
                  - All commit history will be lost.
                  - The files will still exist, but version control will be gone.
                  
                  ---
                  
                  ## 5. What is the difference between working directory, staging area, and repository?
                  
                  Working Directory:
                  The actual files you are editing in your project.
                  
                  Staging Area:
                  A temporary area where changes are prepared before committing.
                  
                  Repository:
                  The local database where Git stores committed snapshots permanently.
                  
                  Flow:
                  Working Directory → Staging Area → Repository
                  ```
                  
                  ---
                  
                  # ✅ Step 3: Stage and Commit This File
                  
                  ```bash
                  git add day-22-notes.md
                  git commit -m "Add Day 22 notes on Git workflow understanding"
                  ```
                  
                  ---
                  
                  # 🧠 What You Just Achieved
                  
                  You now understand:
                  
                  ✔ Why Git has a staging area
                  ✔ How snapshots work
                  ✔ What `.git` actually stores
                  ✔ The internal Git flow
                  
                  This is foundation-level Git knowledge 💪
                  
                  ---
                  
          
        

'''
   

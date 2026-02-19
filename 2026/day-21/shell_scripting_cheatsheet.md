Task 1: Basics
        Document the following with short descriptions and examples:
              Shebang (#!/bin/bash) — what it does and why it matters
              Running a script — chmod +x, ./script.sh, bash script.sh
              Comments — single line (#) and inline
              Variables — declaring, using, and quoting ($VAR, "$VAR", '$VAR')
              Reading user input — read
              Command-line arguments — $0, $1, $#, $@, $?

'''
Perfect 👍 — this is **Shell Scripting Foundation Level**.
I’ll document each concept clearly with short explanations + examples.

---

# ✅ Task 1: Shell Scripting Basics

---

              ## 1️⃣ Shebang (`#!/bin/bash`)
              
              ### 🔹 What It Does
              
              The **shebang** tells the system **which interpreter** should execute the script.
              
              ### 🔹 Why It Matters
              
              Without it, the script may run with the wrong shell (like `sh` instead of `bash`), causing errors.
              
              ### 🔹 Example
              
              ```bash
              #!/bin/bash
              echo "Hello World"
              ```
              
              When you run:
              
              ```
              ./script.sh
              ```
              
              The OS knows to execute it using `/bin/bash`.
              
              ---
              
              ## 2️⃣ Running a Script
              
              There are **two main ways**:
              
              ---
              
              ### 🔹 Method 1: Make it Executable
              
              ```bash
              chmod +x script.sh
              ./script.sh
              ```
              
              * `chmod +x` → gives execute permission
              * `./script.sh` → runs it from current directory
              
              ---
              
              ### 🔹 Method 2: Run with Bash Directly
              
              ```bash
              bash script.sh
              ```
              
              No need for execute permission.
              
              ---
              
              ## 3️⃣ Comments
              
              Comments are ignored by the shell.
              
              ---
              
              ### 🔹 Single-line Comment
              
              ```bash
              # This is a comment
              echo "Hello"
              ```
              
              ---
              
              ### 🔹 Inline Comment
              
              ```bash
              echo "Hello"  # Prints greeting
              ```
              
              ---
              
              ## 4️⃣ Variables
              
              ---
              
              ### 🔹 Declaring a Variable
              
              ```bash
              NAME="Andu"
              AGE=25
              ```
              
              ⚠ No spaces around `=`.
              
              ---
              
              ### 🔹 Using a Variable
              
              ```bash
              echo $NAME
              echo "My name is $NAME"
              ```
              
              ---
              
              ### 🔹 Quoting Differences
              
              ```bash
              echo "$NAME"   # Expands variable
              echo '$NAME'   # Prints literal $NAME
              ```
              
              Example:
              
              ```
              Andu
              $NAME
              ```
              
              ---
              
              ### 🔹 Why Double Quotes Matter
              
              ```bash
              FILE="My File.txt"
              echo "$FILE"   # Correct
              ```
              
              Without quotes, spaces break arguments.
              
              ---
              
              ## 5️⃣ Reading User Input (`read`)
              
              Used to take input during runtime.
              
              ---
              
              ### 🔹 Example
              
              ```bash
              #!/bin/bash
              
              echo "Enter your name:"
              read NAME
              
              echo "Hello $NAME"
              ```
              
              ---
              
              ### 🔹 Inline Prompt
              
              ```bash
              read -p "Enter your age: " AGE
              echo "You are $AGE years old"
              ```
              
              ---
              
              ## 6️⃣ Command-Line Arguments
              
              When passing values while running the script.
              
              Example:
              
              ```
              ./script.sh apple mango
              ```
              
              ---
              
              ### 🔹 Special Variables
              
              | Variable | Meaning                     |
              | -------- | --------------------------- |
              | `$0`     | Script name                 |
              | `$1`     | First argument              |
              | `$2`     | Second argument             |
              | `$#`     | Total number of arguments   |
              | `$@`     | All arguments               |
              | `$?`     | Exit status of last command |
              
              ---
              
              ### 🔹 Example Script
              
              ```bash
              #!/bin/bash
              
              echo "Script name: $0"
              echo "First argument: $1"
              echo "Second argument: $2"
              echo "Total arguments: $#"
              echo "All arguments: $@"
              ```
              
              Run:
              
              ```
              ./script.sh apple mango
              ```
              
              Output:
              
              ```
              Script name: ./script.sh
              First argument: apple
              Second argument: mango
              Total arguments: 2
              All arguments: apple mango
              ```
              
              ---
              
              ### 🔹 Exit Status (`$?`)
              
              ```bash
              ls file.txt
              echo $?
              ```
              
              * `0` → Success
              * Non-zero → Error
              
              Example:
              
              ```
              ls: cannot access file.txt: No such file
              2
              ```
              
              ---

'''

'''
### Task 2: Operators and Conditionals
Document with examples:
1. String comparisons — `=`, `!=`, `-z`, `-n`
2. Integer comparisons — `-eq`, `-ne`, `-lt`, `-gt`, `-le`, `-ge`
3. File test operators — `-f`, `-d`, `-e`, `-r`, `-w`, `-x`, `-s`
4. `if`, `elif`, `else` syntax
5. Logical operators — `&&`, `||`, `!`
6. Case statements — `case ... esac`

   '''
   Perfect 👍 — now we move to **Operators & Conditionals in Bash**.
Clear explanations + practical examples (DevOps-style).

---

# ✅ Task 2: Operators and Conditionals

---

          # 1️⃣ String Comparisons
          
          Used inside `[ ]` (test command).
          
          ---
          
          ## 🔹 `=` (equal)
          
          ```bash
          NAME="Andu"
          
          if [ "$NAME" = "Andu" ]; then
              echo "Name matches"
          fi
          ```
          
          ---
          
          ## 🔹 `!=` (not equal)
          
          ```bash
          if [ "$NAME" != "Admin" ]; then
              echo "Not admin user"
          fi
          ```
          
          ---
          
          ## 🔹 `-z` (string is empty)
          
          ```bash
          VAR=""
          
          if [ -z "$VAR" ]; then
              echo "Variable is empty"
          fi
          ```
          
          ---
          
          ## 🔹 `-n` (string is NOT empty)
          
          ```bash
          VAR="Hello"
          
          if [ -n "$VAR" ]; then
              echo "Variable is not empty"
          fi
          ```
          
          ---
          
          # 2️⃣ Integer Comparisons
          
          Used for numeric checks.
          
          ---
          
          ## 🔹 `-eq` (equal)
          
          ```bash
          NUM=10
          
          if [ "$NUM" -eq 10 ]; then
              echo "Equal"
          fi
          ```
          
          ---
          
          ## 🔹 `-ne` (not equal)
          
          ```bash
          if [ "$NUM" -ne 5 ]; then
              echo "Not equal"
          fi
          ```
          
          ---
          
          ## 🔹 `-lt` (less than)
          
          ```bash
          if [ "$NUM" -lt 20 ]; then
              echo "Less than 20"
          fi
          ```
          
          ---
          
          ## 🔹 `-gt` (greater than)
          
          ```bash
          if [ "$NUM" -gt 5 ]; then
              echo "Greater than 5"
          fi
          ```
          
          ---
          
          ## 🔹 `-le` (less than or equal)
          
          ```bash
          if [ "$NUM" -le 10 ]; then
              echo "Less or equal to 10"
          fi
          ```
          
          ---
          
          ## 🔹 `-ge` (greater than or equal)
          
          ```bash
          if [ "$NUM" -ge 10 ]; then
              echo "Greater or equal to 10"
          fi
          ```
          
          ---
          
          # 3️⃣ File Test Operators
          
          Very important in DevOps scripting.
          
          ---
          
          ## 🔹 `-f` → regular file exists
          
          ```bash
          if [ -f "app.log" ]; then
              echo "File exists"
          fi
          ```
          
          ---
          
          ## 🔹 `-d` → directory exists
          
          ```bash
          if [ -d "archive" ]; then
              echo "Directory exists"
          fi
          ```
          
          ---
          
          ## 🔹 `-e` → file or directory exists
          
          ```bash
          if [ -e "config.yaml" ]; then
              echo "Exists"
          fi
          ```
          
          ---
          
          ## 🔹 `-r` → readable
          
          ```bash
          if [ -r "app.log" ]; then
              echo "Readable file"
          fi
          ```
          
          ---
          
          ## 🔹 `-w` → writable
          
          ```bash
          if [ -w "app.log" ]; then
              echo "Writable file"
          fi
          ```
          
          ---
          
          ## 🔹 `-x` → executable
          
          ```bash
          if [ -x "deploy.sh" ]; then
              echo "Executable script"
          fi
          ```
          
          ---
          
          ## 🔹 `-s` → file not empty
          
          ```bash
          if [ -s "app.log" ]; then
              echo "File is not empty"
          fi
          ```
          
          ---
          
          # 4️⃣ if, elif, else Syntax
          
          ---
          
          ## 🔹 Basic Syntax
          
          ```bash
          if [ condition ]; then
              # code
          elif [ condition ]; then
              # code
          else
              # code
          fi
          ```
          
          ---
          
          ## 🔹 Example
          
          ```bash
          read -p "Enter number: " NUM
          
          if [ "$NUM" -gt 100 ]; then
              echo "Large number"
          elif [ "$NUM" -eq 100 ]; then
              echo "Exactly 100"
          else
              echo "Small number"
          fi
          ```
          
          ---
          
          # 5️⃣ Logical Operators
          
          ---
          
          ## 🔹 `&&` (AND)
          
          ```bash
          if [ -f "app.log" ] && [ -r "app.log" ]; then
              echo "File exists and is readable"
          fi
          ```
          
          ---
          
          ## 🔹 `||` (OR)
          
          ```bash
          if [ "$USER" = "root" ] || [ "$USER" = "admin" ]; then
              echo "Privileged user"
          fi
          ```
          
          ---
          
          ## 🔹 `!` (NOT)
          
          ```bash
          if [ ! -f "app.log" ]; then
              echo "File does not exist"
          fi
          ```
          
          ---
          
          # 6️⃣ Case Statements (`case ... esac`)
          
          Used instead of multiple `if-elif`.
          
          ---
          
          ## 🔹 Syntax
          
          ```bash
          case "$variable" in
              pattern1)
                  command
                  ;;
              pattern2)
                  command
                  ;;
              *)
                  default_command
                  ;;
          esac
          ```
          
          ---
          
          ## 🔹 Example (DevOps Menu Style)
          
          ```bash
          read -p "Enter environment (dev/test/prod): " ENV
          
          case "$ENV" in
              dev)
                  echo "Deploying to development"
                  ;;
              test)
                  echo "Deploying to testing"
                  ;;
              prod)
                  echo "Deploying to production"
                  ;;
              *)
                  echo "Invalid environment"
                  ;;
          esac
          ```
          
          ---
          
          # 🔥 Real DevOps Example Combining Everything
          
          ```bash
          #!/bin/bash
          
          FILE="app.log"
          
          if [ -f "$FILE" ] && [ -s "$FILE" ]; then
              echo "Valid log file"
          else
              echo "File missing or empty"
              exit 1
          fi
          ```
          ---
   '''
'''


'''
### Task 3: Loops
Document with examples:
1. `for` loop — list-based and C-style
2. `while` loop
3. `until` loop
4. Loop control — `break`, `continue`
5. Looping over files — `for file in *.log`
6. Looping over command output — `while read line`

   '''
    Excellent 👌 — now we move to **Loops in Bash** (very important for automation & DevOps).

Clear explanation + practical examples.

---

          # ✅ Task 3: Loops in Bash
          
          ---
          
          # 1️⃣ `for` Loop
          
          ---
          
          ## 🔹 A) List-Based `for` Loop
          
          Used to iterate over a list of items.
          
          ### Example 1: Simple List
          
          ```bash
          for NAME in Alice Bob Charlie
          do
              echo "Hello $NAME"
          done
          ```
          
          Output:
          
          ```
          Hello Alice
          Hello Bob
          Hello Charlie
          ```
          
          ---
          
          ### Example 2: Range of Numbers
          
          ```bash
          for i in {1..5}
          do
              echo "Number: $i"
          done
          ```
          
          Output:
          
          ```
          Number: 1
          Number: 2
          Number: 3
          Number: 4
          Number: 5
          ```
          
          ---
          
          ### Example 3 (DevOps-style): Loop through servers
          
          ```bash
          for SERVER in server1 server2 server3
          do
              echo "Checking $SERVER"
          done
          ```
          
          ---
          
          ## 🔹 B) C-Style `for` Loop
          
          Similar to C/Java syntax.
          
          ### Syntax:
          
          ```bash
          for (( initialization; condition; increment ))
          do
              commands
          done
          ```
          
          ---
          
          ### Example:
          
          ```bash
          for (( i=1; i<=5; i++ ))
          do
              echo "Count: $i"
          done
          ```
          
          ---
          
          # 2️⃣ `while` Loop
          
          Runs **while condition is true**.
          
          ---
          
          ### Example 1: Counter
          
          ```bash
          COUNT=1
          
          while [ "$COUNT" -le 5 ]
          do
              echo "Count: $COUNT"
              COUNT=$((COUNT + 1))
          done
          ```
          
          ---
          
          ### Example 2: Infinite Loop
          
          ```bash
          while true
          do
              echo "Running..."
              sleep 2
          done
          ```
          
          (Press `Ctrl+C` to stop)
          
          ---
          
          # 3️⃣ `until` Loop
          
          Opposite of `while`.
          
          Runs **until condition becomes true**.
          
          ---
          
          ### Example:
          
          ```bash
          COUNT=1
          
          until [ "$COUNT" -gt 5 ]
          do
              echo "Count: $COUNT"
              COUNT=$((COUNT + 1))
          done
          ```
          
          Explanation:
          
          * Runs until COUNT > 5
          * Stops when condition becomes true
          
          ---
          
          # 4️⃣ Loop Control
          
          ---
          
          ## 🔹 `break` (Exit Loop)
          
          ```bash
          for i in {1..10}
          do
              if [ "$i" -eq 5 ]; then
                  break
              fi
              echo "$i"
          done
          ```
          
          Output:
          
          ```
          1
          2
          3
          4
          ```
          
          ---
          
          ## 🔹 `continue` (Skip Current Iteration)
          
          ```bash
          for i in {1..5}
          do
              if [ "$i" -eq 3 ]; then
                  continue
              fi
              echo "$i"
          done
          ```
          
          Output:
          
          ```
          1
          2
          4
          5
          ```
          
          ---
          
          # 5️⃣ Looping Over Files
          
          Very common in log automation.
          
          ---
          
          ## Example:
          
          ```bash
          for file in *.log
          do
              echo "Processing $file"
          done
          ```
          
          Explanation:
          
          * `*.log` expands to all log files
          * Iterates one by one
          
          ---
          
          ## Safer Version (Handles No Files Case)
          
          ```bash
          shopt -s nullglob
          
          for file in *.log
          do
              echo "Processing $file"
          done
          ```
          
          Without `nullglob`, `*.log` may return literal string if no match.
          
          ---
          
          # 6️⃣ Looping Over Command Output
          
          ⚠ Very important: Avoid `for line in $(command)` for multi-word lines.
          
          ---
          
          ## ❌ Wrong Way (Breaks Spaces)
          
          ```bash
          for line in $(cat app.log)
          do
              echo "$line"
          done
          ```
          
          This splits by spaces.
          
          ---
          
          ## ✅ Correct Way: `while read`
          
          ```bash
          while read -r line
          do
              echo "$line"
          done < app.log
          ```
          
          ---
          
          ## Example: Process Log Lines
          
          ```bash
          while read -r line
          do
              if [[ "$line" == *ERROR* ]]; then
                  echo "Error found: $line"
              fi
          done < app.log
          ```
          
          ---
          
          ## Loop Over Command Output (Pipe Version)
          
          ```bash
          grep "ERROR" app.log | while read -r line
          do
              echo "Error: $line"
          done
          ```
          
          ---
          
          # 🔥 Real DevOps Example
          
          Process multiple logs and count errors:
          
          ```bash
          for file in *.log
          do
              echo "Analyzing $file"
              
              ERROR_COUNT=$(grep -c "ERROR" "$file")
              echo "Errors in $file: $ERROR_COUNT"
          done
          ```
          ---
   '''
'''

### Task 4: Functions
Document with examples:
1. Defining a function — `function_name() { ... }`
2. Calling a function
3. Passing arguments to functions — `$1`, `$2` inside functions
4. Return values — `return` vs `echo`
5. Local variables — `local`

'''
Perfect 👌 — now we move to **Functions in Bash** (very important for clean, modular scripts).

Clear explanation + practical examples.

---


---

              # 1️⃣ Defining a Function
              
              ---
              
              ## 🔹 Basic Syntax
              
              ```bash
              function_name() {
                  commands
              }
              ```
              
              OR (also valid):
              
              ```bash
              function function_name {
                  commands
              }
              ```
              
              ---
              
              ## 🔹 Example
              
              ```bash
              greet() {
                  echo "Hello, welcome!"
              }
              ```
              
              This only defines the function. It does not run yet.
              
              ---
              
              # 2️⃣ Calling a Function
              
              To execute the function, just write its name:
              
              ```bash
              greet
              ```
              
              ---
              
              ## Full Example
              
              ```bash
              #!/bin/bash
              
              greet() {
                  echo "Hello, welcome!"
              }
              
              greet
              ```
              
              Output:
              
              ```
              Hello, welcome!
              ```
              
              ---
              
              # 3️⃣ Passing Arguments to Functions
              
              Functions accept arguments just like scripts.
              
              Inside functions:
              
              * `$1` → first argument
              * `$2` → second argument
              * `$#` → number of arguments
              
              ---
              
              ## 🔹 Example
              
              ```bash
              add() {
                  echo "First number: $1"
                  echo "Second number: $2"
              }
              
              add 10 20
              ```
              
              Output:
              
              ```
              First number: 10
              Second number: 20
              ```
              
              ---
              
              ## 🔹 Example (DevOps Style)
              
              ```bash
              check_service() {
                  SERVICE=$1
                  systemctl status "$SERVICE"
              }
              
              check_service nginx
              ```
              
              ---
              
              # 4️⃣ Return Values — `return` vs `echo`
              
              This is very important.
              
              ---
              
              ## 🔹 `return`
              
              * Used for exit status (0–255 only)
              * Similar to script exit code
              
              ```bash
              check_number() {
                  if [ "$1" -gt 10 ]; then
                      return 0   # success
                  else
                      return 1   # failure
                  fi
              }
              
              check_number 15
              echo "Exit status: $?"
              ```
              
              Output:
              
              ```
              Exit status: 0
              ```
              
              ---
              
              ### Important:
              
              `return` cannot return strings or large numbers.
              
              ---
              
              ## 🔹 `echo` (To Return Actual Data)
              
              Used when you want to return a value.
              
              ```bash
              add() {
                  result=$(( $1 + $2 ))
                  echo "$result"
              }
              
              SUM=$(add 5 7)
              echo "Sum is $SUM"
              ```
              
              Output:
              
              ```
              Sum is 12
              ```
              
              ---
              
              ## 🔥 Difference Summary
              
              | Method   | Used For                  |
              | -------- | ------------------------- |
              | `return` | Exit status (0 = success) |
              | `echo`   | Returning actual data     |
              
              ---
              
              # 5️⃣ Local Variables (`local`)
              
              By default, variables in Bash are global.
              
              Use `local` inside functions to restrict scope.
              
              ---
              
              ## ❌ Without `local`
              
              ```bash
              name="Global"
              
              change_name() {
                  name="Changed"
              }
              
              change_name
              echo "$name"
              ```
              
              Output:
              
              ```
              Changed
              ```
              
              The global variable got modified.
              
              ---
              
              ## ✅ With `local`
              
              ```bash
              name="Global"
              
              change_name() {
                  local name="Changed"
                  echo "Inside function: $name"
              }
              
              change_name
              echo "Outside function: $name"
              ```
              
              Output:
              
              ```
              Inside function: Changed
              Outside function: Global
              ```
              
              ---
              
              # 🔥 Real DevOps Example (Clean Structure)
              
              ```bash
              #!/bin/bash
              
              check_file() {
                  local FILE=$1
              
                  if [ -f "$FILE" ]; then
                      echo "File exists"
                      return 0
                  else
                      echo "File missing"
                      return 1
                  fi
              }
              
              check_file app.log
              
              if [ $? -eq 0 ]; then
                  echo "Proceeding with analysis..."
              else
                  echo "Stopping script."
              fi
              ```
              
              ---
              
              # 🎯 Best Practices for Functions
              
              ✔ Always quote variables
              ✔ Use `local` inside functions
              ✔ Use `return` for status
              ✔ Use `echo` for actual output
              ✔ Keep functions small and reusable

---

'''

### Task 5: Text Processing Commands
Document the most useful flags/patterns for each:
1. `grep` — search patterns, `-i`, `-r`, `-c`, `-n`, `-v`, `-E`
2. `awk` — print columns, field separator, patterns, `BEGIN/END`
3. `sed` — substitution, delete lines, in-place edit
4. `cut` — extract columns by delimiter
5. `sort` — alphabetical, numerical, reverse, unique
6. `uniq` — deduplicate, count
7. `tr` — translate/delete characters
8. `wc` — line/word/char count
9. `head` / `tail` — first/last N lines, follow mode

'''
    Excellent 👌 — this is **Core Linux Text Processing** — extremely important for DevOps, SRE, log analysis, and automation.

Clear documentation + practical examples.

---

          # ✅ 1️⃣ `grep` — Search Patterns
          
          Used to search text inside files.
          
          ---
          
          ## 🔹 Basic Search
          
          ```bash
          grep "ERROR" app.log
          ```
          
          ---
          
          ## 🔹 Useful Flags
          
          | Flag | Meaning           |
          | ---- | ----------------- |
          | `-i` | Case insensitive  |
          | `-r` | Recursive search  |
          | `-c` | Count matches     |
          | `-n` | Show line numbers |
          | `-v` | Invert match      |
          | `-E` | Extended regex    |
          
          ---
          
          ### 🔹 Case-Insensitive
          
          ```bash
          grep -i "error" app.log
          ```
          
          ---
          
          ### 🔹 Count Matches
          
          ```bash
          grep -c "ERROR" app.log
          ```
          
          ---
          
          ### 🔹 Show Line Numbers
          
          ```bash
          grep -n "CRITICAL" app.log
          ```
          
          ---
          
          ### 🔹 Invert Match (Exclude)
          
          ```bash
          grep -v "INFO" app.log
          ```
          
          ---
          
          ### 🔹 Extended Regex
          
          ```bash
          grep -E "ERROR|FAILED" app.log
          ```
          
          ---
          
          # ✅ 2️⃣ `awk` — Field Processing
          
          Powerful for column-based processing.
          
          ---
          
          ## 🔹 Print Columns
          
          ```bash
          awk '{print $1}' app.log
          ```
          
          Prints first column.
          
          ---
          
          ## 🔹 Field Separator (`-F`)
          
          Example CSV:
          
          ```bash
          awk -F',' '{print $2}' data.csv
          ```
          
          ---
          
          ## 🔹 Pattern Matching
          
          ```bash
          awk '/ERROR/ {print $0}' app.log
          ```
          
          ---
          
          ## 🔹 BEGIN / END Blocks
          
          ```bash
          awk '
          BEGIN {print "Start"}
          /ERROR/ {count++}
          END {print "Total Errors:", count}
          ' app.log
          ```
          
          ---
          
          # ✅ 3️⃣ `sed` — Stream Editor
          
          Used for modifying text.
          
          ---
          
          ## 🔹 Substitution
          
          ```bash
          sed 's/ERROR/WARNING/' app.log
          ```
          
          Replaces first match per line.
          
          ---
          
          ## 🔹 Replace All Occurrences
          
          ```bash
          sed 's/ERROR/WARNING/g' app.log
          ```
          
          ---
          
          ## 🔹 Delete Lines
          
          ```bash
          sed '/DEBUG/d' app.log
          ```
          
          ---
          
          ## 🔹 Delete Specific Line Number
          
          ```bash
          sed '3d' app.log
          ```
          
          ---
          
          ## 🔹 In-Place Edit
          
          ```bash
          sed -i 's/old/new/g' file.txt
          ```
          
          ⚠ This modifies the file directly.
          
          ---
          
          # ✅ 4️⃣ `cut` — Extract Columns
          
          ---
          
          ## 🔹 By Delimiter
          
          ```bash
          cut -d',' -f2 data.csv
          ```
          
          * `-d` → delimiter
          * `-f` → field number
          
          ---
          
          ## 🔹 By Character Position
          
          ```bash
          cut -c1-5 file.txt
          ```
          
          Extracts characters 1 to 5.
          
          ---
          
          # ✅ 5️⃣ `sort` — Sort Data
          
          ---
          
          ## 🔹 Alphabetical
          
          ```bash
          sort names.txt
          ```
          
          ---
          
          ## 🔹 Numerical
          
          ```bash
          sort -n numbers.txt
          ```
          
          ---
          
          ## 🔹 Reverse
          
          ```bash
          sort -r names.txt
          ```
          
          ---
          
          ## 🔹 Unique (Combine with uniq)
          
          ```bash
          sort file.txt | uniq
          ```
          
          ---
          
          # ✅ 6️⃣ `uniq` — Remove Duplicates
          
          ⚠ Must be sorted first.
          
          ---
          
          ## 🔹 Remove Duplicates
          
          ```bash
          sort file.txt | uniq
          ```
          
          ---
          
          ## 🔹 Count Occurrences
          
          ```bash
          sort file.txt | uniq -c
          ```
          
          ---
          
          ## 🔹 Show Only Duplicates
          
          ```bash
          sort file.txt | uniq -d
          ```
          
          ---
          
          # ✅ 7️⃣ `tr` — Translate / Delete Characters
          
          ---
          
          ## 🔹 Convert to Uppercase
          
          ```bash
          echo "hello" | tr 'a-z' 'A-Z'
          ```
          
          ---
          
          ## 🔹 Delete Characters
          
          ```bash
          echo "hello123" | tr -d '0-9'
          ```
          
          ---
          
          ## 🔹 Replace Spaces with Underscore
          
          ```bash
          echo "hello world" | tr ' ' '_'
          ```
          
          ---
          
          # ✅ 8️⃣ `wc` — Word Count
          
          ---
          
          ## 🔹 Line Count
          
          ```bash
          wc -l app.log
          ```
          
          ---
          
          ## 🔹 Word Count
          
          ```bash
          wc -w file.txt
          ```
          
          ---
          
          ## 🔹 Character Count
          
          ```bash
          wc -c file.txt
          ```
          
          ---
          
          # ✅ 9️⃣ `head` / `tail`
          
          ---
          
          ## 🔹 First N Lines
          
          ```bash
          head -n 5 app.log
          ```
          
          ---
          
          ## 🔹 Last N Lines
          
          ```bash
          tail -n 5 app.log
          ```
          
          ---
          
          ## 🔹 Follow Mode (Live Logs)
          
          ```bash
          tail -f app.log
          ```
          
          Used for real-time log monitoring.
          
          ---
          
          # 🔥 Real DevOps Log Analysis Example
          
          Top 5 Errors:
          
          ```bash
          grep "ERROR" app.log \
          | awk -F'ERROR:' '{print $2}' \
          | sort \
          | uniq -c \
          | sort -nr \
          | head -5
          ```
          
          ---
          
          # 🎯 Quick Command Comparison
          
          | Command   | Best For                 |
          | --------- | ------------------------ |
          | grep      | Searching patterns       |
          | awk       | Column processing        |
          | sed       | Editing text             |
          | cut       | Simple column extraction |
          | sort      | Sorting data             |
          | uniq      | Removing duplicates      |
          | tr        | Character conversion     |
          | wc        | Counting                 |
          | head/tail | Previewing files         |
          
          ---
          
          # 🚀 Why This Matters in DevOps
          
          These commands power:
          
          * Log analysis
          * Kubernetes troubleshooting
          * CI/CD debugging
          * Security investigations
          * Metrics extraction
          * Automation scripts


'''

### Task 6: Useful Patterns and One-Liners
Include at least 5 real-world one-liners you find useful. Examples:
- Find and delete files older than N days
- Count lines in all `.log` files
- Replace a string across multiple files
- Check if a service is running
- Monitor disk usage with alerts
- Parse CSV or JSON from command line
- Tail a log and filter for errors in real time

  '''
  Excellent 🔥 — this is where **real DevOps power** starts.

Here are **practical, production-ready one-liners** you’ll actually use in Linux, CI/CD, and troubleshooting.

---

# ✅ Task 6: Useful Patterns & One-Liners

---

## 1️⃣ Find & Delete Files Older Than N Days

### 🔹 Delete `.log` files older than 7 days

```bash
find /var/log -type f -name "*.log" -mtime +7 -exec rm -f {} \;
```

### 🔹 Safer (Preview First)

```bash
find /var/log -type f -name "*.log" -mtime +7
```

Explanation:

* `-mtime +7` → older than 7 days
* `-type f` → files only

---

## 2️⃣ Count Lines in All `.log` Files

```bash
wc -l *.log
```

### 🔹 Total Combined Count

```bash
wc -l *.log | tail -1
```

---

## 3️⃣ Replace a String Across Multiple Files

### 🔹 Replace in All `.conf` Files

```bash
sed -i 's/localhost/127.0.0.1/g' *.conf
```

### 🔹 Recursive Replacement

```bash
grep -rl "localhost" . | xargs sed -i 's/localhost/127.0.0.1/g'
```

Explanation:

* `-r` → recursive
* `-l` → list filenames

---

## 4️⃣ Check if a Service is Running

```bash
systemctl is-active --quiet nginx && echo "Running" || echo "Not running"
```

---

### 🔹 Alternative (Process Check)

```bash
pgrep nginx > /dev/null && echo "Running" || echo "Stopped"
```

---

## 5️⃣ Monitor Disk Usage with Alert

### 🔹 Alert if Usage > 80%

```bash
df -h | awk '$5+0 > 80 {print "High usage on", $6, ":", $5}'
```

---

### 🔹 Exit Script if Threshold Crossed

```bash
THRESHOLD=80
USAGE=$(df / | awk 'NR==2 {print $5}' | tr -d '%')

[ "$USAGE" -gt "$THRESHOLD" ] && echo "Disk Critical!" && exit 1
```

---

## 6️⃣ Parse CSV from Command Line

### 🔹 Print 2nd Column from CSV

```bash
awk -F',' '{print $2}' data.csv
```

---

### 🔹 Sum a Numeric Column

```bash
awk -F',' '{sum += $3} END {print sum}' data.csv
```

---

## 7️⃣ Parse JSON (If `jq` Installed)

### 🔹 Extract Field

```bash
jq '.status' file.json
```

---

### 🔹 Get Nested Value

```bash
jq '.data.user.name' file.json
```

---

## 8️⃣ Tail Log and Filter Errors in Real Time

```bash
tail -f app.log | grep --line-buffered "ERROR"
```

⚠ `--line-buffered` ensures real-time output.

---

## 9️⃣ Top 5 Most Frequent IPs in Log

```bash
awk '{print $1}' access.log | sort | uniq -c | sort -nr | head -5
```

---

## 🔟 Kill Process by Port Number

```bash
kill -9 $(lsof -t -i:8080)
```

---

## 1️⃣1️⃣ Check Memory Usage Quickly

```bash
free -m | awk 'NR==2 {printf "Memory Usage: %.2f%%\n", $3*100/$2 }'
```

---

## 1️⃣2️⃣ Find Large Files (>500MB)

```bash
find / -type f -size +500M 2>/dev/null
```

---

# 🔥 Bonus DevOps One-Liner

## Restart Service If Down

```bash
systemctl is-active --quiet nginx || systemctl restart nginx
```

---

# 🎯 Real Production Combo Example

Alert if disk > 85% AND send mail (example logic):

```bash
df -h | awk '$5+0 > 85 {print $6 " is above threshold"}'
```

---

# 🚀 Why These Matter

These are used in:

* Cron jobs
* CI/CD validation
* Log monitoring
* Incident response
* Kubernetes node troubleshooting
* Security audits

---

  '''

### Task 7: Error Handling and Debugging
Document with examples:
1. Exit codes — `$?`, `exit 0`, `exit 1`
2. `set -e` — exit on error
3. `set -u` — treat unset variables as error
4. `set -o pipefail` — catch errors in pipes
5. `set -x` — debug mode (trace execution)
6. Trap — `trap 'cleanup' EXIT`


'''
              ---
              
              # ✅ Task 7: Error Handling & Debugging in Bash
              
              ---
              
              # 1️⃣ Exit Codes
              
              Every command returns an exit status:
              
              * `0` → Success
              * Non-zero → Failure
              
              ---
              
              ## 🔹 `$?` — Get Last Command Exit Code
              
              ```bash
              ls file.txt
              echo $?
              ```
              
              If file exists → `0`
              If not → `2` (or non-zero)
              
              ---
              
              ## 🔹 `exit` Command
              
              Used to terminate script with status.
              
              ```bash
              exit 0   # Success
              exit 1   # Generic failure
              ```
              
              ---
              
              ## 🔹 Example Script
              
              ```bash
              #!/bin/bash
              
              if [ ! -f "app.log" ]; then
                  echo "File not found"
                  exit 1
              fi
              
              echo "File exists"
              exit 0
              ```
              
              ---
              
              # 2️⃣ `set -e` — Exit on Error
              
              Stops script immediately if any command fails.
              
              ---
              
              ## 🔹 Without `set -e`
              
              ```bash
              #!/bin/bash
              
              ls missing.txt
              echo "This still runs"
              ```
              
              Even if `ls` fails, script continues.
              
              ---
              
              ## 🔹 With `set -e`
              
              ```bash
              #!/bin/bash
              set -e
              
              ls missing.txt
              echo "This will NOT run"
              ```
              
              Script exits immediately.
              
              ---
              
              ### ⚠ Important
              
              `set -e` does NOT exit if:
              
              * Command is inside `if`
              * Command is part of `||`
              * Command is part of `&&`
              
              ---
              
              # 3️⃣ `set -u` — Treat Unset Variables as Error
              
              Prevents silent bugs.
              
              ---
              
              ## 🔹 Without `set -u`
              
              ```bash
              echo "$NAME"
              ```
              
              If `NAME` is unset → prints empty.
              
              ---
              
              ## 🔹 With `set -u`
              
              ```bash
              set -u
              echo "$NAME"
              ```
              
              Error:
              
              ```
              unbound variable
              ```
              
              This helps detect typos.
              
              ---
              
              # 4️⃣ `set -o pipefail`
              
              Normally, pipelines only return the exit code of the **last command**.
              
              ---
              
              ## 🔹 Without `pipefail`
              
              ```bash
              false | true
              echo $?
              ```
              
              Output:
              
              ```
              0
              ```
              
              Because `true` succeeded.
              
              ---
              
              ## 🔹 With `pipefail`
              
              ```bash
              set -o pipefail
              
              false | true
              echo $?
              ```
              
              Output:
              
              ```
              1
              ```
              
              Now it catches failure inside the pipe.
              
              ---
              
              # 5️⃣ `set -x` — Debug Mode
              
              Prints each command before executing.
              
              ---
              
              ## 🔹 Example
              
              ```bash
              set -x
              
              NAME="Andu"
              echo "$NAME"
              ```
              
              Output:
              
              ```
              + NAME=Andu
              + echo Andu
              Andu
              ```
              
              ---
              
              ## 🔹 Turn Debug On/Off
              
              ```bash
              set -x   # Enable
              set +x   # Disable
              ```
              
              Useful for debugging specific sections.
              
              ---
              
              # 6️⃣ `trap` — Cleanup on Exit
              
              Runs a function when script exits.
              
              ---
              
              ## 🔹 Basic Syntax
              
              ```bash
              trap 'command' EXIT
              ```
              
              ---
              
              ## 🔹 Example: Cleanup Temporary File
              
              ```bash
              #!/bin/bash
              
              TEMP_FILE=$(mktemp)
              
              cleanup() {
                  echo "Cleaning up..."
                  rm -f "$TEMP_FILE"
              }
              
              trap cleanup EXIT
              
              echo "Writing to temp file"
              echo "Hello" > "$TEMP_FILE"
              
              echo "Script running..."
              ```
              
              Even if script fails or is interrupted → cleanup runs.
              
              ---
              
              ## 🔹 Trap Multiple Signals
              
              ```bash
              trap cleanup EXIT INT TERM
              ```
              
              * `EXIT` → script exits
              * `INT` → Ctrl+C
              * `TERM` → termination signal
              
              ---
              
              # 🔥 Production-Ready Template
              
              This is how serious DevOps scripts start:
              
              ```bash
              #!/bin/bash
              
              set -euo pipefail
              
              cleanup() {
                  echo "Cleaning resources..."
              }
              
              trap cleanup EXIT
              
              echo "Starting script..."
              
              # Your logic here
              ```
              
              ---
              
              # 🎯 What `set -euo pipefail` Means
              
              | Option        | Purpose                    |
              | ------------- | -------------------------- |
              | `-e`          | Exit on error              |
              | `-u`          | Unset variables are errors |
              | `-o pipefail` | Catch pipe failures        |
              
              This combination prevents:
              
              * Silent failures
              * Hidden bugs
              * Partial script execution
              
              ---
              
              # 🚀 Real DevOps Example
              
              ```bash
              #!/bin/bash
              set -euo pipefail
              
              FILE="app.log"
              
              if [ ! -f "$FILE" ]; then
                  echo "File missing"
                  exit 1
              fi
              
              ERROR_COUNT=$(grep -c "ERROR" "$FILE")
              
              if [ "$ERROR_COUNT" -gt 10 ]; then
                  echo "Too many errors!"
                  exit 2
              fi
              
              echo "System healthy"
              ```
              
              ---
              
              # 🧠 Summary
              
              You now understand:
              
              ✔ Exit codes
              ✔ Safe termination
              ✔ Strict mode (`set -euo pipefail`)
              ✔ Debug tracing
              ✔ Signal handling with trap
              
              ---
              
              # 🔥 Next Advanced Topics
              
              👉 Arrays
              👉 Background jobs & parallel execution
              👉 Cron automation
              👉 Lock files (prevent duplicate runs)
              👉 Building a production CLI tool framework
              👉 Converting Bash tool into a CI/CD pipeline step
              
              What do you want next? 🚀

'''

### Task 8: Bonus — Quick Reference Table
Create a summary table like this at the top of your cheat sheet:

| Topic | Key Syntax | Example |
|-------|-----------|---------|
| Variable | `VAR="value"` | `NAME="DevOps"` |
| Argument | `$1`, `$2` | `./script.sh arg1` |
| If | `if [ condition ]; then` | `if [ -f file ]; then` |
| For loop | `for i in list; do` | `for i in 1 2 3; do` |
| Function | `name() { ... }` | `greet() { echo "Hi"; }` |
| Grep | `grep pattern file` | `grep -i "error" log.txt` |
| Awk | `awk '{print $1}' file` | `awk -F: '{print $1}' /etc/passwd` |
| Sed | `sed 's/old/new/g' file` | `sed -i 's/foo/bar/g' config.txt` |

'''
Perfect 👌 — here’s a **clean Quick Reference Table** you can place at the top of your Bash cheat sheet.

---

              # 🧠 Bash & Linux Quick Reference Table
              
              | Topic           | Key Syntax                | Example                              |          |      |   |              |
              | --------------- | ------------------------- | ------------------------------------ | -------- | ---- | - | ------------ |
              | Shebang         | `#!/bin/bash`             | `#!/bin/bash`                        |          |      |   |              |
              | Variable        | `VAR="value"`             | `NAME="DevOps"`                      |          |      |   |              |
              | Use Variable    | `$VAR`, `"$VAR"`          | `echo "$NAME"`                       |          |      |   |              |
              | Argument        | `$1`, `$2`, `$#`          | `./script.sh arg1 arg2`              |          |      |   |              |
              | Exit Code       | `$?`, `exit 1`            | `echo $?`                            |          |      |   |              |
              | If              | `if [ condition ]; then`  | `if [ -f file ]; then`               |          |      |   |              |
              | If-Else         | `if ... elif ... else`    | `if [ $x -gt 5 ]; then`              |          |      |   |              |
              | For loop        | `for i in list; do`       | `for i in 1 2 3; do`                 |          |      |   |              |
              | C-Style For     | `for ((i=0;i<5;i++)); do` | `for ((i=1;i<=5;i++)); do`           |          |      |   |              |
              | While           | `while [ condition ]; do` | `while [ $x -lt 5 ]; do`             |          |      |   |              |
              | Until           | `until [ condition ]; do` | `until [ $x -gt 5 ]; do`             |          |      |   |              |
              | Break           | `break`                   | `if [ $i -eq 3 ]; then break; fi`    |          |      |   |              |
              | Continue        | `continue`                | `if [ $i -eq 3 ]; then continue; fi` |          |      |   |              |
              | Function        | `name() { ... }`          | `greet() { echo "Hi"; }`             |          |      |   |              |
              | Local Var       | `local VAR=value`         | `local count=0`                      |          |      |   |              |
              | Read Input      | `read VAR`                | `read -p "Enter: " NAME`             |          |      |   |              |
              | File Check      | `-f`, `-d`, `-e`          | `if [ -f file ]; then`               |          |      |   |              |
              | Integer Compare | `-eq`, `-gt`, `-lt`       | `if [ $x -gt 10 ]; then`             |          |      |   |              |
              | String Compare  | `=`, `!=`, `-z`, `-n`     | `if [ -z "$VAR" ]; then`             |          |      |   |              |
              | Logical AND     | `&&`                      | `[ -f file ] && echo OK`             |          |      |   |              |
              | Logical OR      | `                         |                                      | `        | `cmd |   | echo Failed` |
              | Grep            | `grep pattern file`       | `grep -i "error" log.txt`            |          |      |   |              |
              | Awk             | `awk '{print $1}' file`   | `awk -F: '{print $1}' /etc/passwd`   |          |      |   |              |
              | Sed             | `sed 's/old/new/g' file`  | `sed -i 's/foo/bar/g' config.txt`    |          |      |   |              |
              | Cut             | `cut -d',' -f2`           | `cut -d: -f1 /etc/passwd`            |          |      |   |              |
              | Sort            | `sort -n`, `sort -r`      | `sort -nr numbers.txt`               |          |      |   |              |
              | Uniq            | `uniq -c`                 | `sort file                           | uniq -c` |      |   |              |
              | WC              | `wc -l`, `wc -w`          | `wc -l log.txt`                      |          |      |   |              |
              | Tail            | `tail -f file`            | `tail -f app.log`                    |          |      |   |              |
              | Strict Mode     | `set -euo pipefail`       | `set -euo pipefail`                  |          |      |   |              |
              | Debug Mode      | `set -x`                  | `set -x`                             |          |      |   |              |
              | Trap            | `trap 'cleanup' EXIT`     | `trap cleanup EXIT`                  |          |      |   |              |
              
              ---
              
              # 🔥 Pro Tip
              
              At the top of serious scripts, use:
              
              ```bash
              #!/bin/bash
              set -euo pipefail
              ```
              
              That single line eliminates 80% of common scripting bugs.
              
              ---
              
              If you want, I can now:
              
              * 📄 Convert everything into a printable PDF cheat sheet
              * 📦 Turn this into a structured DevOps Bash Handbook
              * 🧪 Create 20 interview questions based on this
              * 🛠 Build a final mini-project combining everything
              
              What’s next? 🚀

'''




### Task 1: Basic Functions
1. Create `functions.sh` with:
   - A function `greet` that takes a name as argument and prints `Hello, <name>!`
   - A function `add` that takes two numbers and prints their sum
   - Call both functions from the script
  
     '''
           #!/bin/bash
          # Function to greet a user
          greet() {
              name="$1"
              echo "Hello, $name!"
          }
          
          # Function to add two numbers
          add() {
              num1="$1"
              num2="$2"
              sum=$((num1 + num2))
              echo "Sum: $sum"
          }
          
          # Calling the functions
          greet "DevOps"
          add 10 20
  
        
        Hello, DevOps!
        Sum: 30

     '''

### Task 2: Functions with Return Values
1. Create `disk_check.sh` with:
   - A function `check_disk` that checks disk usage of `/` using `df -h`
   - A function `check_memory` that checks free memory using `free -h`
   - A main section that calls both and prints the results
  
     '''
      #!/bin/bash
      # Function to check disk usage of root (/)
      check_disk() {
          echo "===== Disk Usage (/) ====="
          df -h / | awk 'NR==1 || NR==2'
          echo
      }
      
      # Function to check memory usage
      check_memory() {
          echo "===== Memory Usage ====="
          free -h
          echo
      }
      
      # Main section
      echo "System Health Check"
      echo "---------------------"
      
      check_disk
      check_memory
      
      echo "Check completed."
  
        OUTUT:
        System Health Check
        ---------------------
        ===== Disk Usage (/) =====
        Filesystem      Size  Used Avail Use% Mounted on
        /dev/sda1        40G   15G   23G  40% /
        
        ===== Memory Usage =====
                      total        used        free      shared  buff/cache   available
        Mem:           7.7G        2.1G        3.8G        200M        1.8G        5.1G
        Swap:          2.0G          0B        2.0G
        
        Check completed.

     '''

### Task 3: Strict Mode — `set -euo pipefail`
1. Create `strict_demo.sh` with `set -euo pipefail` at the top
2. Try using an **undefined variable** — what happens with `set -u`?
3. Try a command that **fails** — what happens with `set -e`?
4. Try a **piped command** where one part fails — what happens with `set -o pipefail`?

   '''
    #!/bin/bash

    # Enable strict mode
    set -euo pipefail
    
    echo "Strict mode enabled."
    
    echo "---- 1️⃣ Undefined variable demo ----"
    # This will cause failure because variable is not defined
    echo "Value of MY_VAR is: $MY_VAR"
    
    echo "---- 2️⃣ Command failure demo ----"
    # This command will fail (file does not exist)
    ls /nonexistent-directory
    
    echo "---- 3️⃣ Pipe failure demo ----"
    # First command fails, but pipefail will catch it
    cat /nonexistent-file | grep "test"
    
    echo "This line will not execute if any error occurs."



   ----
        1️⃣ set -u → Undefined Variable
        
        When script hits:
        
        echo "$MY_VAR"
        
        
        You get:
        
        bash: MY_VAR: unbound variable
        
        
        ✅ Script exits immediately.
        Because set -u treats undefined variables as errors.

        2️⃣ set -e → Command Failure
        
        When script runs:
        
        ls /nonexistent-directory
        
        
        You get:
        
        ls: cannot access '/nonexistent-directory': No such file or directory
        
        
        ✅ Script exits immediately.
        Because set -e stops execution on any non-zero exit status.

        3️⃣ set -o pipefail → Pipe Failure Detection
        
        Normally:
        
        cat missing.txt | grep test
        
        
        Without pipefail:
        
        If grep succeeds, script may continue.
        
        Failure in first command may be ignored.
        
        With pipefail:
        
        If any command in pipeline fails, whole pipeline fails.
        
        Script exits immediately.

        🎯 Why DevOps Engineers Use set -euo pipefail
        Option	Purpose
        -e	Exit on any command failure
        -u	Fail on undefined variables
        -o pipefail	Catch hidden errors in pipelines
        
        This prevents:
        
        Silent failures
        
        Partial deployments
        
        Broken automation
        
        Hard-to-debug CI/CD issues
        
        🚀 Production Best Practice Header
        
        Most production scripts start with:

        #!/bin/bash
        set -euo pipefail
        IFS=$'\n\t'
        
        That’s called Bash strict mode.
        If you want, I can show a real CI/CD failure example where missing pipefail caused production outage — very common interview discussion.
   ----

   '''
### Task 4: Local Variables
1. Create `local_demo.sh` with:
   - A function that uses `local` keyword for variables
   - Show that `local` variables don't leak outside the function
   - Compare with a function that uses regular variables
  
     '''
      #!/bin/bash

      echo "===== Demonstrating local vs global variables ====="
      
      # Function using local variable
      local_function() {
          local message="I am LOCAL"
          echo "Inside local_function: $message"
      }
      
      # Function using regular (global) variable
      global_function() {
          message="I am GLOBAL"
          echo "Inside global_function: $message"
      }
      
      echo "Before calling functions:"
      echo "message = ${message:-undefined}"
      echo
      
      # Call local function
      local_function
      echo "After local_function call:"
      echo "message = ${message:-undefined}"
      echo
      
      # Call global function
      global_function
      echo "After global_function call:"
      echo "message = ${message:-undefined}"
      echo
  
        OUTPUT:
     ===== Demonstrating local vs global variables =====
      Before calling functions:
      message = undefined
      
      Inside local_function: I am LOCAL
      After local_function call:
      message = undefined
      
      Inside global_function: I am GLOBAL
      After global_function call:
      message = I am GLOBAL
  
       Explanation
        🔹 local keyword
           Variable exists only inside the function       
        It does NOT modify global scope
        
        🔹 Regular variable
        Becomes global by default
        Value persists after function execution

     '''
     
   '''

   '''
   ### Task 5: Build a Script — System Info Reporter
            Create `system_info.sh` that uses functions for everything:
            1. A function to print **hostname and OS info**
            2. A function to print **uptime**
            3. A function to print **disk usage** (top 5 by size)
            4. A function to print **memory usage**
            5. A function to print **top 5 CPU-consuming processes**
            6. A `main` function that calls all of the above with section headers
            7. Use `set -euo pipefail` at the top
            
            Output should look clean and readable.

'''
            #!/bin/bash
            
            # Strict mode
            set -euo pipefail
            
            # ------------------------------
            # Function: Hostname & OS Info
            # ------------------------------
            print_system_info() {
                echo "=============================="
                echo " Hostname & OS Information"
                echo "=============================="
                echo "Hostname : $(hostname)"
                echo "OS       : $(uname -o)"
                echo "Kernel   : $(uname -r)"
                echo
            }
            
            # ------------------------------
            # Function: Uptime
            # ------------------------------
            print_uptime() {
                echo "=============================="
                echo " System Uptime"
                echo "=============================="
                uptime -p
                echo
            }
            
            # ------------------------------
            # Function: Disk Usage (Top 5 by Size)
            # ------------------------------
            print_disk_usage() {
                echo "=============================="
                echo " Disk Usage (Top 5 Largest)"
                echo "=============================="
                df -h --total | sort -hr -k2 | head -n 6
                echo
            }
            
            # ------------------------------
            # Function: Memory Usage
            # ------------------------------
            print_memory_usage() {
                echo "=============================="
                echo " Memory Usage"
                echo "=============================="
                free -h
                echo
            }
            
            # ------------------------------
            # Function: Top 5 CPU Processes
            # ------------------------------
            print_top_cpu() {
                echo "=============================="
                echo " Top 5 CPU Consuming Processes"
                echo "=============================="
                ps -eo pid,comm,%cpu --sort=-%cpu | head -n 6
                echo
            }
            
            # ------------------------------
            # Main Function
            # ------------------------------
            main() {
                echo "########################################"
                echo "        SYSTEM INFORMATION REPORT       "
                echo "########################################"
                echo
            
                print_system_info
                print_uptime
                print_disk_usage
                print_memory_usage
                print_top_cpu
            
                echo "Report Generated Successfully."
            }
            
            # Execute main
            main


            **OUTPUT:**
            ########################################
                SYSTEM INFORMATION REPORT
            ########################################
            
            ==============================
             Hostname & OS Information
            ==============================
            Hostname : ip-10-0-0-12
            OS       : GNU/Linux
            Kernel   : 5.15.0-78-generic
            
            ==============================
             System Uptime
            ==============================
            up 2 hours, 14 minutes
            
            ==============================
             Disk Usage (Top 5 Largest)
            ==============================
            Filesystem      Size  Used Avail Use%
            /dev/sda1        40G   20G   18G  53%
            ...
            
            ==============================
             Memory Usage
            ==============================
                          total        used        free
            Mem:           7.7G        2.1G        3.8G
            Swap:          2.0G          0B        2.0G
            
            ==============================
             Top 5 CPU Consuming Processes
            ==============================
            PID   COMMAND   %CPU
            1023  java      35.2
            2101  python    18.4
...
'''
'''


     

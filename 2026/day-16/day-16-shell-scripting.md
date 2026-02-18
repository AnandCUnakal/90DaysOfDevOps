### Task 1: Your First Script
      1. Create a file `hello.sh`
      2. Add the shebang line `#!/bin/bash` at the top
      3. Print `Hello, DevOps!` using `echo`
      4. Make it executable and run it

          azureuser@tws-azure-vm:~/16day$ vi hello.sh
          azureuser@tws-azure-vm:~/16day$ chmod +x hello.sh
          azureuser@tws-azure-vm:~/16day$ ./hello.sh
          ./hello.sh: line 1: !#/bin/bash: No such file or directory
          Hello, Devops

        azureuser@tws-azure-vm:~/16day$ cat hello.sh
          !#/bin/bash
          echo "Hello, Devops"


### Task 2: Variables
1. Create `variables.sh` with:
   - A variable for your `NAME`
   - A variable for your `ROLE` (e.g., "DevOps Engineer")
   - Print: `Hello, I am <NAME> and I am a <ROLE>`
            azureuser@tws-azure-vm:~/16day$ vi variable.sh
            azureuser@tws-azure-vm:~/16day$ chmod +x variable.sh
            azureuser@tws-azure-vm:~/16day$ ./variable.sh
             ./variable.sh: line 1: !#/bin/bash: No such file or directory
            Hello, I am Atharv and I am a Devops Engr

     2. Try using single quotes vs double quotes — what's the difference?
            azureuser@tws-azure-vm:~/16day$ ./variable.sh
            ./variable.sh: line 1: !#/bin/bash: No such file or directory
            Hello, I am $NAME and I am a $ROLE

     azureuser@tws-azure-vm:~/16day$ cat variable.sh
          !#/bin/bash
          NAME="Atharv"
          ROLE="Devops Engr"
          echo "Hello, I am $NAME and I am a $ROLE"


### Task 3: User Input with read
1. Create `greet.sh` that:
   - Asks the user for their name using `read`
   - Asks for their favourite tool
   - Prints: `Hello <name>, your favourite tool is <tool>`

          azureuser@tws-azure-vm:~/16day$ vi greet.sh
          azureuser@tws-azure-vm:~/16day$ chmod +x greet.sh
          azureuser@tws-azure-vm:~/16day$ ./greet.sh
          Enter your name:Atharv
          Which is your Favourite tool:Terraform
          Hello Atharv, your favourite tool is Terraform

    azureuser@tws-azure-vm:~/16day$ cat greet.sh
        !#/bin/bash
        read -p "Enter your name:" NAME
        read -p "Which is your Favourite tool:" FAVT
        echo "Hello $NAME, your favourite tool is $FAVT"

 ### Task 4: If-Else Conditions
1. Create `check_number.sh` that:
   - Takes a number using `read`
   - Prints whether it is **positive**, **negative**, or **zero**

        Here’s your script:
      
      ```bash
      #!/bin/bash
      
      # Ask user to enter a number
      read -p "Enter a number: " num
      
      # Check if number is positive, negative, or zero
      if [ "$num" -gt 0 ]; then
          echo "The number is positive."
      elif [ "$num" -lt 0 ]; then
          echo "The number is negative."
      else
          echo "The number is zero."
      fi
      ```
      
      ---
      
      ## ✅ How to Use
      
      1. Create the file:
      
      ```bash
      vi check_number.sh
      ```
      
      2. Paste the script and save.
      
      3. Give execute permission:
      
      ```bash
      chmod +x check_number.sh
      ```
      
      4. Run it:
      
      ```bash
      ./check_number.sh
      ```

          ---
          
          ### 🔎 Example Output
          
          ```
          Enter a number: 5
          The number is positive.
          ```
          
          ```
          Enter a number: -3
          The number is negative.
          ```
          
          ```
          Enter a number: 0
          The number is zero.
          ```

---

2. Create `file_check.sh` that:
   - Asks for a filename
   - Checks if the file **exists** using `-f`
   - Prints appropriate message

  azureuser@tws-azure-vm:~/16day$ cat file_checck.sh
    !#/bin/bash
    
    read -p "Enter a file name to checck: " filename
    
    if [ -f "$filename" ]; then
            echo "File exist"
    else
            echo "Doesnt exist"
    fi



           azureuser@tws-azure-vm:~/16day$ ./file_checck.sh
          ./file_checck.sh: line 1: !#/bin/bash: No such file or directory
          Enter a file name to checck: hello.sh
          File exist
     
          azureuser@tws-azure-vm:~/16day$ ./file_checck.sh
          ./file_checck.sh: line 1: !#/bin/bash: No such file or directory
          Enter a file name to checck: abcd
          Doesnt exist
     
          azureuser@tws-azure-vm:~/16day$ ls
          check_number.sh  file_checck.sh  greet.sh  hello.sh  variable.sh

### Task 5: Combine It All
    Create `server_check.sh` that:
    1. Stores a service name in a variable (e.g., `nginx`, `sshd`)
    2. Asks the user: "Do you want to check the status? (y/n)"
    3. If `y` — runs `systemctl status <service>` and prints whether it's **active** or **not**
    4. If `n` — prints "Skipped."
    '''
        azureuser@tws-azure-vm:~/16day$ cat server_check.sh
        #!/bin/bash
        
        read -p "Enter service name: (Like a nginx or ssh etc)" sname
        # Store service name in variable
        #service="nginx"   # Change to sshd or any service if needed
        service=$sname
        
        # Ask user for confirmation
        read -p "Do you want to check the status of $service? (y/n): " choice
        
        if [ "$choice" = "y" ]; then
            # Get service status
            status=$(systemctl is-active $service 2>/dev/null)
        
            if [ "$status" = "active" ]; then
                echo "$service service is ACTIVE."
            else
                echo "$service service is NOT active."
            fi
        
        elif [ "$choice" = "n" ]; then
            echo "Skipped."
        
        else
            echo "Invalid input. Please enter y or n."
        fi

    '''

    azureuser@tws-azure-vm:~/16day$ ./server_check.sh
        Enter service name: (Like a nginx or ssh etc)nginx
        Do you want to check the status of nginx? (y/n): y
        nginx service is ACTIVE.

    azureuser@tws-azure-vm:~/16day$ ./server_check.sh
        Enter service name: (Like a nginx or ssh etc)nginx
        Do you want to check the status of nginx? (y/n): y
        nginx service is NOT active.

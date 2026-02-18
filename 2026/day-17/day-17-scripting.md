  ---

## Challenge Tasks

### Task 1: For Loop
1. Create `for_loop.sh` that:
   - Loops through a list of 5 fruits and prints each one
  
   - azureuser@tws-azure-vm:~/17day$ cat for_loop.sh
        !#/bin/bash
        
        for fruits in apple banana grapes guva kiwi
        do
                echo "The fruits are: $fruits "
        done

    ./for_loop.sh: line 1: !#/bin/bash: No such file or directory
        The fruits are: apple
        The fruits are: banana
        The fruits are: grapes
        The fruits are: guva
        The fruits are: kiwi

2. Create `count.sh` that:
   - Prints numbers 1 to 10 using a for loop
    azureuser@tws-azure-vm:~/17day$ cat countdown.sh
      !#/bin/bash
      for i in {1..10}
      do
              echo $i
      done

      azureuser@tws-azure-vm:~/17day$ ./countdown.sh
        ./countdown.sh: line 1: !#/bin/bash: No such file or directory
        1
        2
        3
        4
        5
        6
        7
        8
        9
        10
---

### Task 2: While Loop
1. Create `countdown.sh` that:
   - Takes a number from the user
   - Counts down to 0 using a while loop
   - Prints "Done!" at the end
      azureuser@tws-azure-vm:~/17day$ cat whilelo.sh
          #!/bin/bash
          # Take number from user
          read -p "Enter a number to start countdown: " num
          # Countdown using while loop
          while [ "$num" -ge 0 ]
          do
              echo $num
              num=$((num - 1))
          done
          echo "Done!"
  
   - 
      azureuser@tws-azure-vm:~/17day$ vi whilelo.sh
          azureuser@tws-azure-vm:~/17day$ chmod +x whilelo.sh
          azureuser@tws-azure-vm:~/17day$ ./whilelo.sh
          Enter a number to start countdown: 3
          3
          2
          1
          0
          Done!

### Task 3: Command-Line Arguments
1. Create `greet.sh` that:
   - Accepts a name as `$1`
   - Prints `Hello, <name>!`
   - If no argument is passed, prints "Usage: ./greet.sh <name>"


    azureuser@tws-azure-vm:~/17day$ cat greet.sh
    #!/bin/bash

    # Check if argument is passed
    if [ -z "$1" ]; then
        echo "Usage: ./greet.sh <name>"
    else
        echo "Hello, $1!"
    fi

    azureuser@tws-azure-vm:~/17day$ ./greet.sh anand
      Hello, anand!
    
2. Create `args_demo.sh` that:
   - Prints total number of arguments (`$#`)
   - Prints all arguments (`$@`)
   - Prints the script name (`$0`)
    azureuser@tws-azure-vm:~/17day$ cat args_demo.sh
        #!/bin/bash
        echo "Script name: $0"
        echo "Total number of arguments: $#"
        echo "All arguments: $@"

     azureuser@tws-azure-vm:~/17day$ ./args_demo.sh apple mango kiwi
        Script name: ./args_demo.sh
        Total number of arguments: 3
        All arguments: apple mango kiwi

### Task 4: Install Packages via Script
1. Create `install_packages.sh` that:
   - Defines a list of packages: `nginx`, `curl`, `wget`
   - Loops through the list
   - Checks if each package is installed (use `dpkg -s` or `rpm -q`)
   - Installs it if missing, skips if already present
   - Prints status for each package

        #!/bin/bash
        
        # List of packages
        packages=("nginx" "curl" "wget")
        
        # Detect package manager
        if command -v dpkg >/dev/null 2>&1; then
            pkg_check="dpkg -s"
            pkg_install="sudo apt-get install -y"
        elif command -v rpm >/dev/null 2>&1; then
            pkg_check="rpm -q"
            if command -v dnf >/dev/null 2>&1; then
                pkg_install="sudo dnf install -y"
            else
                pkg_install="sudo yum install -y"
            fi
        else
            echo "Unsupported package manager."
            exit 1
        fi
        
        # Loop through packages
        for pkg in "${packages[@]}"
        do
            echo "Checking $pkg..."
        
            if $pkg_check "$pkg" >/dev/null 2>&1; then
                echo "$pkg is already installed. Skipping."
            else
                echo "$pkg is NOT installed. Installing..."
                $pkg_install "$pkg"
        
                if [ $? -eq 0 ]; then
                    echo "$pkg installed successfully."
                else
                    echo "Failed to install $pkg."
                fi
            fi
        
            echo "-----------------------------------"
        done

        azureuser@tws-azure-vm:~/17day$ ./install_packeges.sh
        Checking nginx...
        nginx is already installed. Skipping.
        -----------------------------------
        Checking curl...
        curl is already installed. Skipping.
        -----------------------------------
        Checking wget...
        wget is already installed. Skipping.
        -----------------------------------

  ### Task 5: Error Handling
1. Create `safe_script.sh` that:
   - Uses `set -e` at the top (exit on error)
   - Tries to create a directory `/tmp/devops-test`
   - Tries to navigate into it
   - Creates a file inside
   - Uses `||` operator to print an error if any step fails

  '''
   #!/bin/bash
    # Exit immediately if any command fails
    set -e
    DIR="/tmp/devops-test"
    FILE="test_file.txt"

    # Create directory
    mkdir -p "$DIR" || { echo "Failed to create directory $DIR"; exit 1; }
    
    # Navigate into directory
    cd "$DIR" || { echo "Failed to change directory to $DIR"; exit 1; }
    
    # Create file
    touch "$FILE" || { echo "Failed to create file $FILE"; exit 1; }
    
    echo "All steps completed successfully."

  '''

    azureuser@tws-azure-vm:~/17day$ vi safe_script.sh
          azureuser@tws-azure-vm:~/17day$ chmod +x safe_script.sh
          azureuser@tws-azure-vm:~/17day$ ./safe_script.sh
          All steps completed successfully.
          azureuser@tws-azure-vm:~/17day$ ls /tmp/devops-test/
          test_file.txt

2. Modify your `install_packages.sh` to check if the script is being run as root — exit with a message if not.
   '''
            #!/bin/bash
            
            # Exit if not run as root
            if [ "$EUID" -ne 0 ]; then
                echo "Error: This script must be run as root."
                echo "Please run using: sudo ./install_packages.sh"
                exit 1
            fi
            
            # List of packages
            packages=("nginx" "curl" "wget")
            
            # Detect package manager
            if command -v dpkg >/dev/null 2>&1; then
                pkg_check="dpkg -s"
                pkg_install="apt-get install -y"
            elif command -v rpm >/dev/null 2>&1; then
                pkg_check="rpm -q"
                if command -v dnf >/dev/null 2>&1; then
                    pkg_install="dnf install -y"
                else
                    pkg_install="yum install -y"
                fi
            else
                echo "Unsupported package manager."
                exit 1
            fi
            
            # Loop through packages
            for pkg in "${packages[@]}"
            do
                echo "Checking $pkg..."
            
                if $pkg_check "$pkg" >/dev/null 2>&1; then
                    echo "$pkg is already installed. Skipping."
                else
                    echo "$pkg is NOT installed. Installing..."
                    $pkg_install "$pkg"
            
                    if [ $? -eq 0 ]; then
                        echo "$pkg installed successfully."
                    else
                        echo "Failed to install $pkg."
                    fi
                fi
            
                echo "-----------------------------------"
            done

   '''

   azureuser@tws-azure-vm:~/17day$ ./check_root.sh
      Error: This script must be run as root.
      Please run using: sudo ./install_packages.sh


        

# Day 10 Challenge

## Files Created
devops.txt notes.txt & script.sh

## Permission Changes
added execute permission for script.sh
then removed the execute permission to script.sh

## Commands Used
```
Created Files 
    touch devops.txt
    touch notes.txt
    echo "Hello Devops" > vi script.sh
Check the files permission    
    ls -l
Created Directory 
    mkdir day10
Created script.h file with "Hello dosto" comment
    vi script.sh
    cat script.sh
    ls -l
Adding some contents to notes     
    echo "This day10 assignement tasks" > notes.txt
    cat notes.txt
    ls -l

        azureuser@tws-azure-vm:~/day10$ vi script.sh
        azureuser@tws-azure-vm:~/day10$ cat script.sh
        echo "Hello devops"

        azureuser@tws-azure-vm:~/day10$ ls -l
        total 8
        -rw-rw-r-- 1 azureuser azureuser   0 Feb 17 10:41 devops.txt
        -rw-r--r-- 1 azureuser azureuser 127 Feb 17 10:41 notes.txt
        -rw-rw-r-- 1 azureuser azureuser  20 Feb 17 10:44 script.sh

        azureuser@tws-azure-vm:~/day10$ echo "This day10 assignement tasks" > notes.txt
        azureuser@tws-azure-vm:~/day10$ cat notes.txt
        This day10 assignement tasks
        azureuser@tws-azure-vm:~/day10$ ls -l
        total 8
        -rw-rw-r-- 1 azureuser azureuser  0 Feb 17 10:41 devops.txt
        -rw-r--r-- 1 azureuser azureuser 29 Feb 17 10:46 notes.txt
        -rw-rw-r-- 1 azureuser azureuser 20 Feb 17 10:44 script.sh
        azureuser@tws-azure-vm:~/day10$ cat notes.txt
        This day10 assignement tasks


Display first 5 lines
    cat /etc/passwd | head -n 5
Display last 5 lines
    cat /etc/passwd | tail -n 5

    azureuser@tws-azure-vm:~/day10$ cat /etc/passwd | head -n 5
    root:x:0:0:root:/root:/bin/bash
    daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
    bin:x:2:2:bin:/bin:/usr/sbin/nologin
    sys:x:3:3:sys:/dev:/usr/sbin/nologin
    sync:x:4:65534:sync:/bin:/bin/sync

    azureuser@tws-azure-vm:~/day10$ cat /etc/passwd | tail -n 5
    azureuser:x:1000:1000:Ubuntu:/home/azureuser:/bin/bash
    tokyo:x:1001:1001:,,,:/home/tokyo:/bin/bash
    berlin:x:1002:1002:,,,:/home/berlin:/bin/bash
    professor:x:1003:1003:,,,:/home/professor:/bin/bash
    nairobi:x:1004:1007::/home/nairobi:/bin/sh



Adding execute permission to script.sh
  chmod o+x script.sh
  ls -l

bash script.sh
  chmod 444 devops.txt
  chmod 640 notes.txt

      azureuser@tws-azure-vm:~/day10$ ls -l
      total 12
      -r--r--r-- 1 azureuser azureuser    0 Feb 17 10:41 devops.txt
      -rw-r----- 1 azureuser azureuser   29 Feb 17 10:46 notes.txt
      drwxr-xr-x 2 azureuser azureuser 4096 Feb 17 10:50 project
      -rw-rw-r-x 1 azureuser azureuser   20 Feb 17 10:44 script.sh


created directory 
  mkdir project
  ls -l

  chmod 755 project/
  ls -l

  vi devops.txt
  cat devops.txt
  ls -l
  
  Removing execute permission from script.sh and trying to execute 
  chmod -x script.sh
  ls -l
  bash script.sh
 but execute permission are removed
 After executing this shows the content of script added "bash script.sh"
    azureuser@tws-azure-vm:~/day10$ ls -l
    total 16
    -r--r--r-- 1 azureuser azureuser   98 Feb 17 11:05 devops.txt
    -rw-r----- 1 azureuser azureuser   29 Feb 17 10:46 notes.txt
    drwxr-xr-x 2 azureuser azureuser 4096 Feb 17 10:50 project
    -rw-rw-r-- 1 azureuser azureuser   20 Feb 17 10:44 script.sh
    azureuser@tws-azure-vm:~/day10$ cat devops.txt
    this is a read only file so its showing this error as
    Changing a readonly filedasdasdaddasadasda
  

  vi devops.txt
  ls -l
  cat devops.txt
  cat notes.txt
  bash script.sh


```

## What I Learned
[3 key points]



<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1af7471d-70da-4f44-8d44-ba0add1d8634" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fdeaff61-0be4-4239-8d63-b7391b576adc" />








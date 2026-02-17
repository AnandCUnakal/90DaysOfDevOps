# Day 11 Challenge

## Files & Directories Created
created a file devops-file.txt
change the owner of file to berlin/tokyo 
verify "ls -l"

## Ownership Changes

    azureuser@tws-azure-vm:~/day11$ ls -l
    total 0
    -rw-rw-r-- 1 tokyo     developers 0 Feb 17 12:29 devops-file.txt
    -rw-rw-r-- 1 azureuser azureuser  0 Feb 17 12:31 team-notes.txt

    azureuser@tws-azure-vm:~/day11$ sudo chown :heist-team team-notes.txt
    azureuser@tws-azure-vm:~/day11$ ls -l
    total 0
    -rw-rw-r-- 1 tokyo     developers 0 Feb 17 12:29 devops-file.txt
    -rw-rw-r-- 1 azureuser heist-team 0 Feb 17 12:31 team-notes.txt
    

    azureuser@tws-azure-vm:~/day11$ ls -l
    total 4
    -rw-rw-r-- 1 berlin    heist-team    0 Feb 17 12:38 app-logs
    -rw-rw-r-- 1 tokyo     developers    0 Feb 17 12:29 devops-file.txt
    drwxrwxr-x 4 azureuser azureuser  4096 Feb 17 12:39 heist-project
    -rw-rw-r-- 1 professor heist-team    0 Feb 17 12:35 project-config.yaml
    -rw-rw-r-- 1 azureuser heist-team    0 Feb 17 12:31 team-notes.txt

    

Example:
- devops-file.txt: user:user → tokyo:heist-team

## Commands Used
Created devops-file.txt
      touch devops-file.txt
      
Changing owner to tokyo of this file 
      sudo chown tokyo devops-file.txt
      ls -l devops-file.txt

changing group to developers group
      sudo chown :developers devops-file.txt
      ls -l devops-file.txt

creating file team-notes.txt
      touch team-notes.txt
      ls -l

Creating a group heist-team
      groupadd heist-team
      sudo groupadd heist-team
      vi /etc/sudoers

Swithing to tokyo user
      su tokyo
      ls -l
      cat /etc/group
      ls -l
      
 assging team-notes to heist-team group
      sudo chown :heist-team team-notes.txt

Creating projecct-config yaml file
      touch project-config.yaml
      
      ls -l
      sudo chown professor project-config.yaml
      ls -l
      
      sudo chown :heist-team project-config.yaml
      ls -l
      
      touch app-logs
      ls -l
      sudo chown berlin app-logs
      sudo chown :heist-team app-logs
      ls -l

  creting a directory with files using below command
      mkdir -p heist-project/vault
      mkdir -p heist-project/plans
      touch heist-project/vault/gold.txt
      touch heist-project/plans/strategy.conf

  creting  a group
      sudo greoupadd planners
      sudo groupadd planners
      ls -l

  assinging user and group to this directory 
      sudo chown -R professor:planners heist-project/
      ls -l
      ls -lR heist-project/
      
Created a vault & tech team 
      sudo groupadd vault-team
      sudo groupadd tech-team

Create directory 
      mkdir bank-heist
      touch bank-heist/access-codes.txt
      touch bank-heist/blueprints.pdf
      touch bank-heist/escape-plan.txt


      cd bank-heist/
      ls -l
      
      sudo chown tokyo:vault-team access-codes.txt
      sudo chown berlin:tech-team blueprints.pdf
      sudo chown nairobi:vault-team escape-plan.txt
      ls -l
      
      cat /etc/ssh/sshd_config
      cat /etc/ssh/sshd_config | grep ver

<img width="461" height="2582" alt="image" src="https://github.com/user-attachments/assets/2f969695-f855-4a38-b754-31ca0b79a897" />


## What I Learned
[3 key points about file ownership]

# View ownership
ls -l filename

# Change owner only
sudo chown newowner filename

# Change group only
sudo chgrp newgroup filename

# Change both owner and group
sudo chown owner:group filename

# Recursive change (directories)
sudo chown -R owner:group directory/

# Change only group with chown
sudo chown :groupname filename


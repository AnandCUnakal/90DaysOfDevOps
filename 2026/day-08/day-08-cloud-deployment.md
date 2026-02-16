<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/74b4a057-50e9-4eef-9812-dcce9c615f4e" />Documentation Template
Create your day-08-cloud-deployment.md with this structure:

  Using azure cloud created a VM 
  updated the system 
  
**Commands Used**
Used Below commands
  sudo apt update -- it will updates 
  apt list --upgradable -- will show upgradable list 
  systemctl status nginx
  systemctl enable nginx
  systemctl start nginx 
  journalctl -u ngninx -n 50

Using ssh command connected to VM
  Syntax: ssh user@ip_addrs
  Ex: ssh azureuser@172.175.88.217

# On your local machine (new terminal window)
# For AWS:
scp -i your-key.pem ubuntu@<your-instance-ip>:~/nginx-logs.txt .

# For Utho:
scp root@<your-instance-ip>:~/nginx-logs.txt .


**Challenges Faced**
  NSG configuration issue found and got know it will be work by priority bases .
  
**What I Learned**
  Creation of VM and adding NSG to VM, inbound role with port number 88 & 443
  Login into VM using ssh command
  configured nginx, checked the services status and able to access via public ip address with port number
  
  <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/72f5840c-1951-4a76-91f9-1be93202fe61" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d44bc9d3-ce8a-4f74-8b97-06ebb2578a21" />


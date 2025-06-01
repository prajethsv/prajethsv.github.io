# ICT171 Assignment 2 – Cloud Server Setup  
# Murdoch IT Hub  

A creative web project for ICT171 Assignment 2, hosted on an AWS Ubuntu EC2 server using GitHub Pages. This site helps Murdoch IT students explore degrees, clubs, and career resources.

🔗 **Live Website:** https://www.murdochithub.me  

---

## Features
- 🌑 Dark mode toggle (JavaScript)  
- 🎓 Links to Murdoch IT degrees  
- 🕹️ Club details and contact links  
- 🛠️ Hosted via Apache2 on Ubuntu server  
- 🔒 HTTPS via Let’s Encrypt  

---



![image](https://github.com/user-attachments/assets/0a46fe3f-c23f-4b29-9a1c-5c91453946bc)

## Step 0: Creating an Ubuntu Instance in AWS
-Go to AWS Console

-Navigate to EC2 → Launch Instance
-Select Ubuntu Server 22.04 LTS (Free tier eligible)

-Choose a t2.micro instance (free tier)

-Under Key pair (login):
-Create or choose a .pem file (e.g. aws-key.pem)

- Configure its **Security Group** to allow:
  - **SSH** (port 22)  
  - **HTTP** (port 80)  
  - **HTTPS** (port 443)
 
 

## Step 1: Register Your Custom Domain (Student Pack)
1. Sign in to your Namecheap account—use the GitHub Student Developer Pack discount to register a `.me` domain for free.  
2. Verify ownership via the Namecheap dashboard.

---





## Step 2: Launch the Cloud Server
- In AWS Console → **EC2**, launch an **Ubuntu Server** instance.
- Use this command to access your ubuntu instance 
 ```bash
  ssh -i XXXXXX.pem ubuntu@XXXXXXXX
```

 





## Step 3: Install Apache2 Web Server
```bash
sudo apt update
sudo apt install -y apache2
sudo systemctl enable --now apache2
```





## Step 4: Secure the Server with UFW Firewall
- Enabled UFW and allowed OpenSSH and Apache.
- Commands used:
  ```bash
  sudo ufw allow OpenSSH
  sudo ufw allow 'Apache Full'
  sudo ufw enable
  sudo ufw status
  ```
![image](https://github.com/user-attachments/assets/bd355fc6-c966-485a-8d28-749fac9bcda4)





## Step 5: Deploy the Website from GitHub to Apache

- In Namecheap → Domain List → Manage → Advanced DNS.
- Under Host Records, add an A record:
- Type: A

```bash
Host: @  
Value: <YOUR_EC2_IP>        ← e.g. 13.238.217.176  
TTL: Automatic
```

- Add a CNAME record (To get WWW)
  
```bash
Type: CNAME  
Host: www  
Value: murdochithub.me.  
TTL: Automatic
```





## Step 6: Enable HTTPS with Certbot

```bash
sudo apt install -y certbot python3-certbot-apache
sudo certbot --apache -d murdochithub.me -d www.murdochithub.me
```






## Step 7: Deploy the Website from GitHub

- SSH into your EC2 instance:
- Clone your repo and copy files to Apache’s document root:
```bash
cd ~
git clone https://github.com/prajethsv/prajethsv.github.io.git
sudo rsync -av --delete ~/prajethsv.github.io/ /var/www/html/
sudo systemctl reload apache2
```
- Everytime you edit something on your html in github, to update, simply:
```bash
cd ~/prajethsv.github.io
git pull origin main
sudo rsync -av --delete ~/prajethsv.github.io/ /var/www/html/
sudo systemctl reload apache2
```










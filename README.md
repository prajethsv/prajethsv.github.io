# ICT171 Assignment 2 – Cloud Server Setup  
# Murdoch IT Hub  

![image](https://github.com/user-attachments/assets/92b8fa53-9f84-43c7-8f72-2c32ad133bc0)

---

## License

This project is licensed under the [Apache License 2.0](LICENSE).

Murdoch IT Hub is a static educational website hosted on an AWS EC2 Ubuntu server. It was built for ICT171 Assignment 2 to help Murdoch University IT students explore degrees, student clubs, and career opportunities. The site is manually configured using Apache2, GitHub, and a free .me domain from the GitHub Student Developer Pack.



Global IP: 52.62.110.76

DNS Entry: [www.murdochithub.me](https://murdochithub.me )

GitHub Repo: https://github.com/prajethsv/prajethsv.github.io

---

**📽️ Video Explainer**
A full walkthrough of all setup steps is available in the accompanying explainer video, demonstrating:

- AWS EC2 setup and SSH login
- Apache installation and firewall setup
- GitHub repo cloning and deployment
- Domain and DNS configuration
- HTTPS activation using Certbot

**▶️ Access the YouTube Video: ▶️** https://youtu.be/KncMj4w8iRQ 

---

 **⚠️⚠️⚠️ Security Precautions ⚠️⚠️⚠️**

- NEVER share your .pem key publicly.
- Keep your Ubuntu instance updated regularly with sudo apt update && sudo apt upgrade -y.

---

**⏪ Pre-Requisites Summary ⏪**

- AWS account with Free Tier eligibility
- GitHub account for repo hosting
- Namecheap account (GitHub Student Pack)
- Basic knowledge of SSH and Linux commands
- LINK TO W3 SCHOOLS https://www.w3schools.com 

---

## Features
- 🌑 Dark mode toggle (JavaScript)  
- 🎓 Links to Murdoch IT degrees  
- 🕹️ Club details and contact links  
- 🛠️ Hosted via Apache2 on Ubuntu server  
- 🔒 HTTPS via Let’s Encrypt  

---
## 🔓 Step 0: Loggining into AWS
- Before you can create and manage your cloud server, you need to log into your AWS account.

## What you need:
An active AWS account. If you don’t have one, go to aws.amazon.com and sign up for free.

A valid payment method (AWS offers a free tier which is enough for this project).

https://aws.amazon.com/pricing/?nc2=h_ql_pr_ln&aws-products-pricing.sort-by=item.additionalFields.productNameLowercase&aws-products-pricing.sort-order=asc&awsf.Free%20Tier%20Type=*all&awsf.tech-category=*all

![image](https://github.com/user-attachments/assets/daff6d14-bb94-4e3e-ade6-ff3f93736dee)

---


## 🪜 Step 1: Creating an Ubuntu Instance in AWS
- Go to AWS Console

- Navigate to EC2 → Launch Instance
- Select Ubuntu Server 22.04 LTS (Free tier eligible)

- Choose a t2.micro instance (free tier)

- Under Key pair (login):
- Create or choose a .pem file (e.g. aws-key.pem)

- Configure its **Security Group** to allow:
  - **SSH** (port 22)  
  - **HTTP** (port 80)  
  - **HTTPS** (port 443)

 ![image](https://github.com/user-attachments/assets/0a46fe3f-c23f-4b29-9a1c-5c91453946bc)

 ---

## 🔑 Step 2: Register a Free .me Domain (Student Pack)
1. Sign in to your Namecheap account
2. Use the GitHub Student Developer Pack discount to register a `.me` domain for free.  
3. Verify ownership via the Namecheap dashboard.
4. This process takes 3-4 days for them to verify
   
https://www.namecheap.com/myaccount/login/

![image](https://github.com/user-attachments/assets/1db1f4a9-1870-4720-bae7-665602933f07)


---




---
## 🔌 Step 3: SSH into Your Server
- In AWS Console → **EC2**, launch an **Ubuntu Server** instance.
- Use this command to access your ubuntu instance 
 ```bash
  ssh -i XXXXXX.pem ubuntu@XXXXXXXX
```

---
 




---
## 🌐 Step 4: Install Apache2 Web Server
```bash
sudo apt update  # Updates the package list to get latest versions
sudo apt install -y apache2  # Installs Apache web server
sudo systemctl enable --now apache2 # Starts Apache and enables it at boot
```
- If Apache doesn't start: `sudo systemctl restart apache2`
---



---
## 🔐 Step 5: Enable UFW Firewall
- Enabled UFW and allowed OpenSSH and Apache.
- UFW is a firewall tool that helps control which traffic can access your server.
- Commands used:
  ```bash
  sudo ufw allow OpenSSH
  sudo ufw allow 'Apache Full'
  sudo ufw enable
  sudo ufw status
  ```
  - Check firewall with: `sudo ufw status`

![image](https://github.com/user-attachments/assets/bd355fc6-c966-485a-8d28-749fac9bcda4)

---



## 🌍 Step 6: Point Domain to EC2 (DNS Setup)

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

---



## 🔒 Step 7: Enable HTTPS through CertBot


-Install CertBot

```bash
sudo apt update
sudo apt install certbot python3-certbot-apache -y

```
- Run Certbot to Enable HTTPS

```
sudo certbot --apache
```
- If website doesn’t show: Check permissions or verify files exist in `/var/www/html`
- If HTTPS setup fails: Re-run `sudo certbot --apache` or check DNS propagation

---



## 📁 Step 8: Deploy Website from GitHub

- SSH into your EC2 instance:
- Clone your repo and copy files to Apache’s document root:
```bash
cd ~
git clone https://github.com/prajethsv/prajethsv.github.io.git
sudo rsync -av --delete ~/prajethsv.github.io/ /var/www/html/
sudo systemctl reload apache2
```
🔁 Updating the Website Later
```bash

cd ~/[YOURGITHUBIO]
git pull origin main
sudo rsync -av --delete ~/[YOURGITHUB]/ /var/www/html/
sudo systemctl reload apache2
```

- Accessed the live site from multiple devices and browsers (Chrome, Firefox, Mobile)
- Verified HTTPS was working (padlock icon appears in browser)
- Ran `curl -I http://murdochithub.me` to confirm HTTP 200 response
![image](https://github.com/user-attachments/assets/ab37ad7d-04a0-4588-b093-62ed5a9ac5d2)


## 🔄️ Step 9: Script (Auto-Deploy Website To Make Our Life Easier 

Create Empty Script 

```
nano ~/deploy.sh
```

Paste this in your script, replacing the [YOURGITHUBIO]
```
#!/bin/bash
cd /home/ubuntu/[YOURGITHUBIO] || exit 1
git pull origin main
sudo rsync -av --delete /home/ubuntu/[YOURGITHUB]/ /var/www/html/
sudo systemctl reload apache2
```
Make the script run 

```
chmod +x ~/deploy.sh
```


To run program 

```
./deploy.sh
```
---

<<<<<<< HEAD
<<<<<<< HEAD
## 🪞References

-	GitHub Student Developer Pack: https://education.github.com/pack
-	Certbot Setup Guide: https://certbot.eff.org/instructions 
-	Murdoch IT Blog: https://www.murdoch.edu.au/news/blogs/future-in-technology 
-	Template Credits : https://www.w3schools.com/html/default.asp  

=======
>>>>>>> 4944f62 (Initial setup)
=======
>>>>>>> d30c454 (Initial setup)



# ICT171 Assignment 2 – Cloud Server Setup  
## Murdoch IT Hub  

---

## License

This project is licensed under the [Apache License 2.0](LICENSE).

Murdoch IT Hub is a static educational website hosted on an AWS EC2 Ubuntu server. It was built for ICT171 Assignment 2 to help Murdoch University IT students explore degrees, student clubs, and career opportunities. The site is manually configured using Apache2, GitHub, and a free .me domain from the GitHub Student Developer Pack. 



Global IP: 52.62.110.76
- 🔐 Note: The global IP will show a certificate warning due to SSL not applying to raw IPs. Please use the DNS Entry Below for secure access.

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

**▶️ Access the YouTube Video: ▶️** https://youtu.be/KncMj4w8iRQ <---------------------

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
- Link to W3 Schools https://www.w3schools.com 

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

[View Interface](https://github.com/user-attachments/assets/daff6d14-bb94-4e3e-ade6-ff3f93736dee)

An active AWS account. If you don’t have one, go to [aws.amazon.com](https://shorturl.at/KXD5V 
) and sign up for free.

A valid payment method (AWS offers a free tier which is enough for this project).

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

[View Screenshot](https://github.com/user-attachments/assets/0a46fe3f-c23f-4b29-9a1c-5c91453946bc)

 ---

## 🔑 Step 2: Register a Free .me Domain (Student Pack)
1. Sign in to your Namecheap account
2. Use the GitHub Student Developer Pack discount to register a `.me` domain for free.  
3. Verify ownership via the Namecheap dashboard.
4. This process takes 3-4 days for them to verify
   
https://www.namecheap.com/myaccount/login/

[View Screenshot](https://github.com/user-attachments/assets/1db1f4a9-1870-4720-bae7-665602933f07)


---

## 🔌 Step 3: SSH into Your Server
- In AWS Console → **EC2**, launch an **Ubuntu Server** instance.
- Use this command to access your ubuntu instance 
 ```bash
  ssh -i XXXXXX.pem ubuntu@XXXXXXXX
```

---

## 🌐 Step 4: Install Apache2 Web Server
```bash
sudo apt update  # Updates the package list to get latest versions
sudo apt install -y apache2  # Installs Apache web server
sudo systemctl enable --now apache2 # Starts Apache and enables it at boot
```
- If Apache doesn't start: `sudo systemctl restart apache2`

  
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

[View Screenshot](https://github.com/user-attachments/assets/bd355fc6-c966-485a-8d28-749fac9bcda4)

---

## ➕ Step 6: Setting up an Elastic IP Address
To ensure a consistent and public-facing IP address, an Elastic IP was set up and associated with the EC2 instance.

Steps:

- In the AWS EC2 dashboard, under "Elastic IPs", click "Allocate Elastic IP address"
- Keep the default options and click "Allocate"
- After allocation, click "Actions" → "Associate Elastic IP address"
- Choose your EC2 instance from the dropdown and associate the Elastic IP
- Confirm that the IP is now bound to your running server
- Outcome: My server now has a static public IP: 52.62.110.76, which remains the same even after restarts, this is perfect for DNS mapping and public access.


---


## 🌍 Step 7: Point Domain to EC2 (DNS Setup)

- In Namecheap → Domain List → Manage → Advanced DNS.
- Under Host Records, add an A record:
- Type: A

```bash
Host: @  
Value: <YOUR_EC2_IP>        ← e.g. 52.62.110.76
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
---

## 📜 Step 9: Script (Auto Deploy Website To Make Our Life Easier )
A simple automation script that updates the live website by pulling the latest changes from GitHub, syncing files to the server’s web directory, and restarting Apache—making deployments quickly without the use commands run over and over again.


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

## 🤐 Step 10: Configuring Security Group Inbound Rules
Navigated to the EC2 instance security group settings in the AWS Console.

- Created (or modified) a security group with inbound rules allowing:
- SSH (port 22) from 0.0.0.0/0 — enables remote terminal access from anywhere.
- HTTP (port 80) from 0.0.0.0/0 — allows web traffic to access the website.
- HTTPS (port 443) from 0.0.0.0/0 — enables secure web traffic over TLS/SSL.
  - The security group inbound rules for SSH, HTTP, and HTTPS were configured and applied after initial instance setup but before final testing, ensuring full accessibility.

---

## 🔒 Step 11: Enable HTTPS through CertBot


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


## 🧪✅ Step 12: Testing and Verification

- Accessed the live site from multiple devices and browsers (Chrome, Edge, Mobile)  
  - Mobile Portrait: [View Image](https://github.com/user-attachments/assets/0eb0129c-00c6-4221-9525-d97bce314a76)  
  - Mobile Landscape: [View Image](https://github.com/user-attachments/assets/a4466272-393f-408c-b63b-8ae133b84143)  
  - Chrome: [View Image](https://github.com/user-attachments/assets/62f9ff73-ef9c-455d-b5a0-24d0111e8186)  
  - Microsoft Edge: [View Image](https://github.com/user-attachments/assets/17566ff7-cb7d-423b-ba8d-ca613707951a)

- Verified HTTPS was working (padlock icon appears in browser)
  ![Uploading image.png…]()

- Ran `curl -I http://murdochithub.me` to confirm HTTP 200 response
[View Screenshot](https://github.com/user-attachments/assets/ab37ad7d-04a0-4588-b093-62ed5a9ac5d2)

---

## 🪞References

-	GitHub Student Developer Pack: https://education.github.com/pack
-	Certbot Setup Guide: https://certbot.eff.org/instructions 
-	Murdoch IT Blog: https://www.murdoch.edu.au/news/blogs/future-in-technology 
-	Template Credits : https://www.w3schools.com/html/default.asp  







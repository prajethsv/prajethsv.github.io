# ICT171 Assignment 2 – Cloud Server Setup
# Murdoch IT Hub 💻🔥

A creative web project for ICT171 Assignment 2, hosted on an AWS Ubuntu EC2 server using GitHub Pages. This site helps Murdoch IT students explore degrees, clubs, and career resources.

🔗 **Live Website:** [https://prajethsv.github.io](https://prajethsv.github.io)

## Features
- 🌑 Dark mode toggle (JavaScript)
- 🎓 Links to Murdoch IT degrees
- 🕹️ Club details and contact links
- 🛠️ Hosted via Apache2 on Ubuntu server

## Step 1: Launch the Cloud Server
- Created an Ubuntu Server instance on AWS EC2.
- Configured security groups to allow SSH and HTTP traffic.

## Step 2: Install Apache2 Web Server
- Ran `sudo apt update` and `sudo apt install apache2`.
- Verified Apache was running by visiting the public IP in a browser.

## Step 3: Secure the Server with UFW Firewall
- Enabled UFW and allowed OpenSSH and Apache.
- Commands used:
  ```bash
  sudo ufw allow OpenSSH
  sudo ufw allow 'Apache Full'
  sudo ufw enable
  sudo ufw status
![image](https://github.com/user-attachments/assets/bd355fc6-c966-485a-8d28-749fac9bcda4)

## Step 4: Upload the Website
- Cloned my GitHub repo using `git clone` on the server.
- Replaced the default Apache page with my custom `index.html` file.
- Commands used:
  ```bash
  sudo rm /var/www/html/index.html
  sudo cp index.html /var/www/html/

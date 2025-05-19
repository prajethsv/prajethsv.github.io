# ICT171 Assignment 2 – Cloud Server Setup

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

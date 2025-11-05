# 🖥️ Linux Server Configuration (LAMP Stack + FTP)

A complete end-to-end project to install, configure, and deploy a **Linux-based web server** using the **LAMP Stack (Linux, Apache, MySQL, PHP)** and **FTP (vsftpd)** for file transfer and website management.  
This project is ideal for demonstrating Linux administration and web hosting setup skills.

---

## 📘 Table of Contents
1. [Abstract](#abstract)  
2. [Project Aim](#project-aim)  
3. [Objectives](#objectives)  
4. [System Requirements](#system-requirements)  
5. [Tools & Technologies Used](#tools--technologies-used)  
6. [Implementation (All Commands in One Page)](#implementation-all-commands-in-one-page)  
7. [Testing & Verification](#testing--verification)  
8. [Output Screens (Description)](#output-screens-description)  
9. [Results & Conclusion](#results--conclusion)  
10. [References](#references)  
11. [License](#license)

---

## 🧩 Abstract

This project focuses on configuring a **web hosting server** using the **LAMP stack** (Linux, Apache, MySQL, PHP) and setting up an **FTP server (vsftpd)** for managing files remotely.  
It enables hosting of PHP-based dynamic websites and provides secure access for uploading content.

---

## 🎯 Project Aim

To install and configure a **Linux web server** using the **LAMP stack** and **FTP service**, enabling hosting and management of dynamic web applications.

---

## 🎯 Objectives

- To configure and deploy a web server using **Apache, MySQL, PHP**.  
- To set up **vsftpd** for secure file transfer.  
- To deploy a sample website and verify web hosting functionality.  
- To practice **Linux system administration** commands and service management.  

---

## ⚙️ System Requirements

| Category | Specification |
|-----------|---------------|
| **Operating System** | Ubuntu 22.04 LTS / Debian / Fedora |
| **Processor** | Dual-core CPU |
| **RAM** | 4 GB minimum |
| **Storage** | 20 GB free disk space |
| **Network** | Internet connection required |
| **Software Packages** | Apache2, MySQL/MariaDB, PHP, vsftpd |

---

## 🧰 Tools & Technologies Used

| Tool | Purpose |
|------|----------|
| **Linux (Ubuntu)** | Base operating system |
| **Apache2** | Web server |
| **MySQL/MariaDB** | Database management |
| **PHP** | Server-side scripting |
| **vsftpd** | FTP server |
| **UFW** | Firewall configuration |

---

## 💻 Implementation (All Commands in One Page)

Below is the **complete one-page setup script** that installs and configures LAMP + FTP on Ubuntu.

```bash
#!/bin/bash
# ============================================================
# LINUX SERVER CONFIGURATION (LAMP STACK + FTP SERVER)
# ============================================================

# STEP 1: Update System
sudo apt update && sudo apt upgrade -y

# STEP 2: Install Apache Web Server
sudo apt install apache2 -y
sudo systemctl enable apache2
sudo systemctl start apache2

# STEP 3: Install MySQL Server
sudo apt install mysql-server -y
sudo systemctl enable mysql
sudo systemctl start mysql
sudo mysql_secure_installation

# STEP 4: Install PHP and Dependencies
sudo apt install php libapache2-mod-php php-mysql -y

# STEP 5: Test PHP
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php > /dev/null

# STEP 6: Configure Firewall (UFW)
sudo ufw allow OpenSSH
sudo ufw allow 'Apache Full'
sudo ufw enable

# STEP 7: Install FTP Server (vsftpd)
sudo apt install vsftpd -y
sudo systemctl enable vsftpd
sudo systemctl start vsftpd

# STEP 8: Configure vsftpd
sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.bak
sudo bash -c 'cat > /etc/vsftpd.conf <<EOF
listen=YES
anonymous_enable=NO
local_enable=YES
write_enable=YES
chroot_local_user=YES
allow_writeable_chroot=YES
local_umask=022
user_sub_token=\$USER
local_root=/home/\$USER/ftp
pasv_min_port=40000
pasv_max_port=50000
EOF'

# STEP 9: Restart FTP Service
sudo systemctl restart vsftpd

# STEP 10: Create FTP User
sudo adduser ftpuser
sudo mkdir -p /home/ftpuser/ftp/files
sudo chown nobody:nogroup /home/ftpuser/ftp
sudo chmod a-w /home/ftpuser/ftp
sudo chown ftpuser:ftpuser /home/ftpuser/ftp/files

# STEP 11: Create Sample Website
sudo bash -c 'cat > /var/www/html/index.html <<EOF
<!DOCTYPE html>
<html>
<head><title>My LAMP Server</title></head>
<body>
<h1>Welcome to My LAMP + FTP Server!</h1>
<p>Configured by [Your Name]</p>
</body>
</html>
EOF'

# STEP 12: Restart Apache
sudo systemctl restart apache2

# STEP 13: Display Service Status
echo "------------------------------------------------------------"
echo "SERVICE STATUS:"
sudo systemctl status apache2 | grep Active
sudo systemctl status mysql | grep Active
sudo systemctl status vsftpd | grep Active
echo "------------------------------------------------------------"
echo "LAMP + FTP SERVER CONFIGURATION COMPLETE!"
echo "Access Web Server: http://<your_server_ip>"
echo "Access FTP Server: ftp://<your_server_ip>"
echo "------------------------------------------------------------"

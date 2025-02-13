# Private Cloud with OwnCloud on Ubuntu

## Introduction
With the increasing digitization of data, secure file storage and management have become crucial. Ubuntu, an open-source Linux-based operating system, is widely used for server hosting due to its stability, security, and flexibility. OwnCloud is an open-source solution that allows you to create a private cloud for securely storing, sharing, and synchronizing files, providing an alternative to commercial cloud services like Google Drive or Dropbox.

A private cloud enables businesses and individuals to maintain full control over their data without relying on third-party providers. This ensures better privacy, increased customization, and compliance with data protection regulations.

## Prerequisites
Before starting, ensure you have:
- An Ubuntu 20.04 or 22.04 server.
- Root access or a user with sudo privileges.
- An internet connection.
- A domain name (optional but recommended).

Ubuntu is an ideal choice for hosting OwnCloud due to its compatibility with various server applications and strong community support. OwnCloud is designed to work efficiently on Linux and offers advanced file management features, including encryption and multi-device synchronization.

## Installation Steps

### 1. Update the System
```bash
sudo apt update && sudo apt upgrade -y
```

### 2. Install Apache, MariaDB, and PHP
```bash
sudo apt install apache2 mariadb-server -y
```
Enable and start services:
```bash
sudo systemctl enable --now apache2 mariadb
```
Install PHP and required extensions:
```bash
sudo apt install php php-cli php-mysql php-gd php-json php-curl php-mbstring php-intl php-xml php-zip php-bz2 php-ldap php-imagick php-apcu -y
```

### 3. Configure the Database
Secure MariaDB installation:
```bash
sudo mysql_secure_installation
```
Create the OwnCloud database:
```bash
sudo mysql -u root -p
CREATE DATABASE owncloud;
CREATE USER 'ownclouduser'@'localhost' IDENTIFIED BY 'strongpassword';
GRANT ALL PRIVILEGES ON owncloud.* TO 'ownclouduser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### 4. Install and Configure OwnCloud
Download and extract OwnCloud:
```bash
wget https://download.owncloud.com/server/stable/owncloud-complete-latest.tar.bz2
tar -xjf owncloud-complete-latest.tar.bz2
sudo mv owncloud /var/www/html/
```
Set permissions:
```bash
sudo chown -R www-data:www-data /var/www/html/owncloud
sudo chmod -R 755 /var/www/html/owncloud
```

### 5. Configure Apache
Create a new Apache configuration file:
```bash
sudo nano /etc/apache2/sites-available/owncloud.conf
```
Add the following content:
```
<VirtualHost *:80>
    ServerAdmin admin@your-domain.com
    DocumentRoot /var/www/html/owncloud
    ServerName your-domain.com

    <Directory /var/www/html/owncloud>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/owncloud_error.log
    CustomLog ${APACHE_LOG_DIR}/owncloud_access.log combined
</VirtualHost>
```
Enable the site and restart Apache:
```bash
sudo a2ensite owncloud.conf
sudo a2enmod rewrite headers env dir mime
sudo systemctl restart apache2
```

### 6. Access OwnCloud
Open your browser and navigate to:
```
http://your-server-ip or http://your-domain.com
```
Follow the setup wizard to create an admin account and configure the database.

### 7. Secure with SSL (Let's Encrypt)
Install Certbot:
```bash
sudo apt install certbot python3-certbot-apache -y
```
Generate an SSL certificate:
```bash
sudo certbot --apache -d your-domain.com
```
Set up automatic renewal:
```bash
sudo crontab -e
```
Add the following line:
```
0 3 * * * certbot renew --quiet
```

## Conclusion
This guide details the installation and configuration of OwnCloud on Ubuntu to create a private cloud. Ubuntu is an excellent OS choice due to its robustness and compatibility with OwnCloud.

A private cloud with OwnCloud allows autonomous file management, ensuring security, privacy, and independence from commercial cloud services. By combining Ubuntu's power, OwnCloud's flexibility, and SSL security, you get a reliable and efficient private cloud solution.

## License
This project is licensed under the MIT License - see the LICENSE file for details.

## Author
**Strat** - [Strate-Milambo](https://github.com/Strate-Milambo)

## Contributions
Contributions are welcome! Feel free to fork the repository and submit pull requests.
# Interface
![Mon image locale](mon_image.png)

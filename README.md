# Setting up a LAMP Stack on AWS EC2

This guide will walk you through the process of setting up a LAMP (Linux, Apache, MySQL, PHP) stack on an Amazon Web Services (AWS) EC2 instance.

## Prerequisites

- An AWS account
- Basic knowledge of AWS EC2 and SSH

## Step 1: Launch an EC2 Instance

1. Log in to your AWS Management Console
2. Navigate to EC2 dashboard
3. Click "Launch Instance"
4. Choose an Amazon Linux 2 AMI
5. Select t2.micro instance type (free tier eligible)
6. Configure instance details (use defaults or customize as needed)
7. Add storage (use defaults or customize)
8. Add tags (optional)
9. Configure security group:
   - Allow SSH (port 22) from your IP
   - Allow HTTP (port 80) from anywhere
10. Review and launch
11. Create a new key pair or use an existing one
12. Launch instance

## Step 2: Connect to Your EC2 Instance

```bash
ssh -i /path/to/your-key.pem ec2-user@your-instance-public-dns
```

## Step 3: Update the System

```
sudo apt update 
```

## Step 4: Install Apache Web Server

```
sudo apt install apache2
```

Verify apache2 is running

```
sudo systemctl status apache2
```

Got to your browser and try to access following url

`http://<Public-IP-Address>:80`


## Step 5: Install MySQL

```
sudo apt install mysql-server
```

Secure the MySQL installation:
```
sudo mysql_secure_installation
```


Follow the prompts to set a root password and secure your MySQL installation.

## Step 6: Install PHP

```
sudo apt install php libapache2-mod-php php-mysql
```

## Creating a Virtual Host with Apache

### 1. Set Up Domain Name
Create a directory for your project under /var/www/:
```
sudo mkdir /var/www/projectlamb
```

change ownership

```
sudo chown -R $USER:$USER /var/www/projectlamb
```

### 2. Configuration with Apache
Create a new virtual host configuration file in Apache's sites-available directory:

```
sudo nano /etc/apache2/sites-available/projectlamb.conf
```

Add the following configuration:

```
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    ServerName projectlamb
    ServerAlias www.projectlamb
    DocumentRoot /var/www/projectlamb
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

### 3. Enable Virtual Host
Enable your newly created virtual host:
```
sudo a2ensite projectlamb
```

Disable the default Apache configuration:
```
sudo a2dissite 000-default
```

#### 4. Reload Apache
Ensure changes take effect:
```
sudo systemctl reload apache2
```

#### 5. Testing
Create an index.html file in your project directory:


Access the website via the browser at ```
http://<public-ip-address>:80
```

## Enabling PHP on the Website
### 1. Modify Directory Index
Adjust Apache’s configuration to prioritize PHP files:

```
sudo vi /etc/apache2/mods-enabled/dir.conf
```

Move index.php to the first position:
```
<IfModule mod_dir.c>
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule> 
```

### 2. Reload Apache
Apply the changes:

```
sudo systemctl reload apache2
```

### 3. Create PHP Test Script
Create a simple PHP script to verify the installation:

```
echo "<?php phpinfo(); ?>" > /var/www/projectlamp/index.php
```

### 4. Test PHP Installation
Refresh your page and verify that PHP is working


## Conclusion

You now have a functioning LAMP stack on your AWS EC2 instance. Remember to maintain and secure your server regularly.


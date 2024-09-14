# LAMP Stack Setup in AWS

This documentation outlines the step-by-step setup, configuration, and usage of a LAMP stack (Linux, Apache, MySQL, PHP) on an AWS EC2 instance. This environment is widely used for hosting dynamic websites and web applications.

---

## Prerequisites

1. **EC2 Instance**:
    - Launch an Ubuntu 24.04 LTS EC2 instance using the **t3.micro** type (free tier eligible).
- Create and download a **PEM file** during the launch (you’ll need this to SSH into the instance).
![Create-2c-instance](https://github.com/Prince-Tee/StegHub_Lampstack/blob/main/lampstack%20images/createe2c.PNG)

![Instancerunning](https://github.com/Prince-Tee/StegHub_Lampstack/blob/main/lampstack%20images/running.PNG)






    
    Example SSH command (replace the placeholder values with your actual information):

    ```bash
    ssh -i "lampproject.pem" ubuntu@13.60.179.1
    ```


2. **Security Group Configuration**:
    - In the EC2 dashboard, configure the **inbound rules** of your security group to allow traffic on:
        - **Port 22 (SSH)** – for connecting to the instance.
        - **Port 80 (HTTP)** – for web traffic.
       

---

## Step 1: Install Apache

Apache will serve web pages to users from your web server. Begin by updating the package index and installing Apache.

1. Update your package list:

    ```bash
    sudo apt update
    ```

2. Install Apache:

    ```bash
    sudo apt install apache2
    ```

3. Enable and start Apache:

    ```bash
    sudo systemctl enable apache2
    sudo systemctl start apache2
    ```

4. Verify Apache is running:



    ```bash
    sudo systemctl status apache2
    ```

5. Confirm Apache is serving web pages locally:

    ```bash
    curl http://localhost:80
    ```

    You should see an HTML response from the Apache default page. You can also visit your instance's public IP in a browser:

    ```
    http://<YOUR_INSTANCE_PUBLIC_IP>:80
    ```

    You should see the default Apache landing page ("It works!").

---

## Step 2: Install MySQL

MySQL is the database engine that will store and manage your website’s data.

1. Install MySQL:

    ```bash
    sudo apt install mysql-server
```


2. Secure MySQL installation:

    ```bash
    sudo mysql_secure_installation
    ```

Follow the prompts to configure your root password and remove insecure default settings.



3. Log into MySQL:

    ```bash
    sudo mysql -p
```



    You’ll be prompted to enter the root password you configured.

4. Optionally, if MySQL root access requires updating, you can run:

    ```bash
    ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'yourpassword';
    ```

---

## Step 3: Install PHP

PHP processes code on the server and displays dynamic content to the user. You’ll also need PHP to communicate with MySQL.

1. Install PHP along with necessary modules:

    ```bash
sudo apt install php libapache2-mod-php php-mysql



    ```

2. Verify PHP installation:

    ```bash
    php -v
    ```

    You should see the installed PHP version.

---

## Step 4: Configure Apache Virtual Hosts

You’ll now configure Apache to host your web project under a separate directory.

1. Create a directory for your website:

    ```bash
    sudo mkdir /var/www/projectlamp
    ```

2. Assign ownership of this directory to your current user:

    ```bash
    sudo chown -R $USER:$USER /var/www/projectlamp
    ```

3. Create a new virtual host configuration file:

    ```bash
    sudo vim /etc/apache2/sites-available/projectlamp.conf
    ```

4. Add the following configuration:

    ```apache
    <VirtualHost *:80>
        ServerName projectlamp
        ServerAlias www.projectlamp
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/projectlamp
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
    </VirtualHost>
    ```

5. Enable the new virtual host:

    ```bash
    sudo a2ensite projectlamp.conf
    ```

6. Disable Apache's default virtual host to avoid conflicts:

    ```bash
    sudo a2dissite 000-default.conf
```



7. Check for syntax errors:

    ```bash
    sudo apache2ctl configtest
    ```

8. Reload Apache:

    ```bash
    sudo systemctl reload apache2
    ```
## Step 6: Test HTML Processing

---

DELETE index.html for index.php to show using  rm index.html

## Step 6: Test PHP Processing

1. Navigate to your project directory:

    ```bash
    cd /var/www/projectlamp
    ```

2. Create a basic `index.php` file to test PHP:

    ```bash
    vim index.php
    ```

3. Add the following content:

    ```php
    <?php
    phpinfo();
    ?>
    ```

4. Access this file by visiting your EC2 instance’s IP address:

    ```
    http://<YOUR_INSTANCE_PUBLIC_IP>/index.php
    ```

    You should see a PHP information page, which verifies PHP is working correctly.

5. After verifying, delete the `index.php` file as it contains sensitive information:

    ```bash
    sudo rm index.php
    ```

---



## Conclusion

The LAMP stack provides a reliable environment for hosting dynamic websites and web applications. This guide outlined the essential steps to set up and configure Apache, MySQL, and PHP on an AWS EC2 instance. With this setup, you’re ready to start developing and deploying your web projects.

---

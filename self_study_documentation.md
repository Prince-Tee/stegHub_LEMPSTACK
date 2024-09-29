
### **Self-Study Documentation: Deploying a LEMP Stack on AWS EC2**

#### **Introduction**
This project involved deploying a LEMP stack (Linux, Nginx, MySQL, PHP) on an AWS EC2 instance. It aimed to establish a functioning web server capable of serving dynamic web pages with PHP, managing data using MySQL, and handling requests via Nginx.

#### **Project Setup**
- Launched an EC2 instance and connected via SSH using the command:
  ```
  ssh -i <Your-private-key.pem> ubuntu@<EC2-Public-IP-address>
  ```
- Updated the server’s package index and installed Nginx using:
  ```
  sudo apt update
  sudo apt install nginx
  ```

#### **Configuring Nginx**
- Verified Nginx installation using `sudo systemctl status nginx`.
- Opened port 80 on EC2 to allow web traffic.
- Tested the Nginx server locally using:
  ```
  curl http://localhost:80
  curl http://127.0.0.1:80
  ```
- Accessed the server via the public IP address.

#### **Installing and Configuring MySQL**
- Installed MySQL with:
  ```
  sudo apt install mysql-server
  ```
- Connected to MySQL using:
  ```
  sudo mysql
  ```
- Changed the root password and secured the MySQL installation using:
  ```
  ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
  ```

#### **Errors Encountered**
1. **Error 1045 (28000): Access denied for user 'root'@'localhost'**  
   **Solution:** Resolved by setting the root password correctly using `ALTER USER` in the MySQL shell.

2. **404 Error when accessing the PHP info page in the browser**  
   **Solution:** Realized I had not properly configured Nginx to use PHP. Edited the Nginx configuration file `/etc/nginx/sites-available/projectLEMP` and verified the PHP processor location.

3. **Permission issues when creating/editing files in `/var/www/projectLEMP`**  
   **Solution:** Used `sudo` to open `nano` for editing files and changed ownership of the directory with:
   ```
   sudo chown -R $USER:$USER /var/www/projectLEMP
   ```

#### **Configuring PHP with Nginx**
- Installed PHP-FPM and PHP-MySQL using:
  ```
  sudo apt install php-fpm php-mysql
  ```
- Created a server block configuration file at `/etc/nginx/sites-available/projectLEMP` with PHP settings and enabled it by creating a symbolic link to `/etc/nginx/sites-enabled/`.

#### **Creating the Database and Table**
- Created a database named `example_database` and a user `example_user`.
- Created a `todo_list` table and added sample entries.

#### **Testing PHP Integration**
- Created `info.php` and `todo_list.php` files in `/var/www/projectLEMP/` and verified their functionality by accessing them via the web browser.

#### **Lessons Learned**
- Proper Nginx configuration is crucial for PHP integration.
- Always check file permissions and ownership to avoid editing issues.
- Testing configurations step-by-step helps identify issues early.

#### **Conclusion**
Deploying a LEMP stack requires careful attention to detail, especially in configuring Nginx, PHP, and MySQL. This experience provided valuable insight into web server setup, troubleshooting common errors, and integrating different technologies for a seamless deployment.


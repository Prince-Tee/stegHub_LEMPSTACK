#**Deploying a LEMP Stack on AWS EC2: A Step-by-Step Guide**
---

## 1. Connecting to EC2 Instance
Launch Git Bash and run the following command:
```bash
ssh -i <Your-private-key.pem> ubuntu@<EC2-Public-IP-address>
```
**Screenshot:** ![Connecting to EC2](https://github.com/Prince-Tee/stegHub_LEMPSTACK/blob/main/screenshot1.PNG)

---

## 2. Installing Nginx
```bash
sudo apt update
sudo apt install nginx
sudo systemctl status nginx
```
Open TCP port 80 in EC2 Security Group settings to allow web traffic.

**Screenshot:** ![Nginx Installation](https://github.com/Prince-Tee/stegHub_LEMPSTACK/blob/main/screenshot2.PNG)

---

## 3. Configuring Nginx
Check if Nginx is accessible:
```bash
curl http://localhost:80
curl http://127.0.0.1:80
```
Visit `http://<Public-IP-Address>:80` to verify.

**Screenshot:** ![Nginx Web Page](https://github.com/Prince-Tee/stegHub_LEMPSTACK/blob/main/screenshot3.PNG)

---

## 4. Installing MySQL
```bash
sudo apt install mysql-server
sudo mysql
```
Run the following command to set the root password:
```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
```
**Screenshot:** ![MySQL Installation](https://github.com/Prince-Tee/stegHub_LEMPSTACK/blob/main/screenshot4.PNG)

---

## 5. Securing MySQL
Run the security script:
```bash
sudo mysql_secure_installation
```
Log in to test:
```bash
sudo mysql -p
```
**Screenshot:** ![MySQL Secure Installation](https://github.com/Prince-Tee/stegHub_LEMPSTACK/blob/main/screenshot5.PNG)

---

## 6. Installing PHP
```bash
sudo apt install php-fpm php-mysql
```
**Screenshot:** ![PHP Installation]https://github.com/Prince-Tee/stegHub_LEMPSTACK/blob/main/screenshot6.PNG)

---

## 7. Configuring Nginx to Use PHP Processor
Create the root web directory for your project:
```bash
sudo mkdir /var/www/projectLEMP
sudo chown -R $USER:$USER /var/www/projectLEMP
```
Create and edit the configuration file:
```bash
sudo nano /etc/nginx/sites-available/projectLEMP
```
Paste the configuration:
```nginx
server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
```
Activate and test configuration:
```bash
sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
sudo nginx -t
sudo unlink /etc/nginx/sites-enabled/default
sudo systemctl reload nginx
```
**Screenshot:** ![Nginx Configuration](https://github.com/Prince-Tee/stegHub_LEMPSTACK/blob/main/screenshot7.PNG)

![Nginx Configuration](https://github.com/Prince-Tee/stegHub_LEMPSTACK/blob/main/screenshot7a.PNG)

![Nginx Configuration](https://github.com/Prince-Tee/stegHub_LEMPSTACK/blob/main/screenshot7b.PNG)
---

## 8. Testing PHP with Nginx
Create a PHP info file:
```bash
sudo nano /var/www/projectLEMP/info.php
```
Add the following code:
```php
<?php
phpinfo();
?>
```
Access `http://<Public-IP-Address>/info.php` in your browser.

**Screenshot:** ![PHP Info Page](https://github.com/Prince-Tee/stegHub_LEMPSTACK/blob/main/screenshot8.PNG)

---

## 9. Creating and Accessing MySQL Database
Log in to MySQL and create a database and user:
```bash
sudo mysql -p
CREATE DATABASE example_database;
CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
GRANT ALL ON example_database.* TO 'example_user'@'%';
exit
```
Log in as the new user and create a table:
```bash
mysql -u example_user -p
CREATE TABLE example_database.todo_list (
    item_id INT AUTO_INCREMENT,
    content VARCHAR(255),
    PRIMARY KEY(item_id)
);
```
Insert data into the table:
```sql
INSERT INTO example_database.todo_list (content) VALUES ("My first important item");
INSERT INTO example_database.todo_list (content) VALUES ("My second important item");
```
**Screenshot:** ![MySQL Database Setup](https://github.com/Prince-Tee/stegHub_LEMPSTACK/blob/main/screenshot9.PNG)

---


## 10. Creating `todo_list.php`
Create a PHP file to display the data:
```bash
sudo nano /var/www/projectLEMP/todo_list.php
```
Add the following code:
```php
<?php
$user = "example_user";
$password = "PassWord.1";
$database = "example_database";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}
?>
```
**Screenshot:** ![todo_list.php Creation](https://github.com/Prince-Tee/stegHub_LEMPSTACK/blob/main/screenshot9.PNG)
```

You can then access `http://<Your-EC2-Public-IP>/todo_list.php` in your browser to verify the display of your "TODO" list.
## 11. Final Testing
Access `http://<Your-EC2-Public-IP>/todo_list.php` to view the data retrieved from the MySQL database.

**Screenshot:** ![Final Output](https://github.com/Prince-Tee/stegHub_LEMPSTACK/blob/main/screenshot11.PNG)

---

Congratulations!
In this guide , we have built a flexible foundation for serving PHP websites and applications to your visitors, using Nginx as web server and MySQL as database management system.

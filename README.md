# ProjectTwo
**WEB STACK IMPLEMENTATION (LEMP STACK)**
# STEP 1 – INSTALLING THE NGINX WEB SERVER
1. Using the app for the first time requires updating server's package index by running the following command
`sudo apt update`
2. Then we need to use apt to install Nginx
`sudo apt install nginx`
3. To verify that nginx was successfully installed and is running as a service in Ubuntu, run:
`sudo systemctl status nginx`
4. To check how we can access it locally in our Ubuntu shell, run:
`curl http://localhost:80`
5. To test how Nginx server can respond to requests from the Internet
`http://54.174.150.180:80` The Nginx status was showing as inactive
I ran the following command to stop apache2
`sudo systemctl stop apache2`
Then I ran the following command to start Nginx
`sudo systemctl start nginx` The Nginx status is now showing green active running
We need to curl the localhost to nginx
`curl localhost`

# STEP 2 — INSTALLING MYSQL
1. We use apt to install the software 
`sudo apt install mysql-server`
It shows the software is already installed and running
2. Ran the following command to login to mysql console 
`sudo mysql` but was getting error message "Access denied for user 'root'@'localhost' (using password: NO)"
3. Tried this command `mysql -u root -p` it prompted for password. I was able to login after entering my password
Screenshot is saved in images folder 
4. Exit the MySQL shell with:
`exit`
5. To test if you’re able to log in to the MySQL console by typing:
`sudo mysql -p` then it prompt to enter password
6. To exit the MySQL console, type:
`exit` 

# STEP 3 – INSTALLING PHP
1. To install these 2 packages at once, run:
`sudo apt install php-fpm php-mysql`

# STEP 4 — CONFIGURING NGINX TO USE PHP PROCESSOR
1. I need to create the root web directory for my_domain as follows:
`sudo mkdir /var/www/projectLEMP`
2. Assign ownership of the directory with the $USER environment variable,
`sudo chown -R $USER:$USER /var/www/projectLEMP`
3.  Using nano to open a new configuration file in Nginx’s sites-available directory
`sudo nano /etc/nginx/sites-available/projectLEMP`
4. Activate your configuration by linking to the config file from Nginx’s sites-enabled directory:
`sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/`
5. To test the configuration
`sudo nginx -t`
6. We also need to disable default Nginx host that is currently configured to listen on port 80, for this run:
`sudo unlink /etc/nginx/sites-enabled/default`
7. Reload Nginx to apply the changes:
`sudo systemctl reload nginx`
8. To create an index.html
`sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html`
9. Now go to your browser and try to open your website URL using IP address and DNS
`http://54.174.150.180:80`
`http://http://ec2-54-174-150-180.compute-1.amazonaws.com/`

# STEP 5 – TESTING PHP WITH NGINX
1. Create a test PHP file in document root
`sudo nano /var/www/projectLEMP/info.php`
2.  Paste the following lines into the new file
`<?php
phpinfo()`
To access the page on my web browser
`http://54.174.150.180:80/info.php`

# Step 6 — Retrieving data from MySQL database with PHP
1. First, connect to the MySQL console using the root account
`mysql -u root -p`
2. To create a new database, run the following command from your MySQL console:
mysql> CREATE DATABASE `example_database`;
3. Create a new user and grant him full privileges on the database you have just created
`CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';`
4. Now we need to give this user permission over the example_database database:
`mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';`
5. Now exit the MySQL shell with:
`exit`
6. To test if the new user has the proper permissions by logging in to the MySQL console again
`mysql -u example_user -p`
7. To show the Database, run:
    `SHOW DATABASES;`
8. Next, !’ll create a test table named todo_list. From the MySQL console by running the following statement:
`CREATE TABLE example_database.todo_list (
item_id INT AUTO_INCREMENT,
content VARCHAR(255),
mPRIMARY KEY(item_id)
);`
Inserted a few roles with different values 
`INSERT INTO example_database.todo_list (content) VALUES ("My first important item");`
9. To confirm that the data was successfully saved to your table, run:
`SELECT * FROM example_database.todo_list;`
10. I created a PHP script that will connect to MySQL and query for my content; Create a new PHP file in your custom web root directory
`nano /var/www/projectLEMP/todo_list.php`

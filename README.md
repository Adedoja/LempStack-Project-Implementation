# LempStack-Project-Implementation

## WHAT'S NEEDED:

- Basic Linux Skill
- Cloud Service Provider - AWS, AZURE, DIGITAL OCEAN etc.
- Internet Connection

## STEP 1 --> LAUNCHING EC2 INSTANCE
Launch an instance in the cloud using Ubuntu's package manager.
![](https://github.com/Adedoja/LempStack-Project-Implementation/blob/main/Lempstack%20Files/lemp%20aws.PNG)

SSH into the EC2 instance using your downloaded key and run the

command below to update the list of packages in the package manager.

```
sudo apt update
```

## STEP 2 --> INSTALLING THE NGINX WEB SERVER
In order to display web pages to our site visitors, we are going to employ Nginx, a high-performance web server.

We’ll use the apt package manager to install this package. Following that, you can use apt install to get Nginx installed:

```
sudo apt update
sudo apt install nginx
```

To verify that nginx was successfully installed and is running as a service in Ubuntu, run:

```
sudo systemctl status nginx
```

If it is green and running, then you did everything correctly – you have just launched your first Web Server in the Clouds!

![](https://github.com/Adedoja/LempStack-Project-Implementation/blob/main/Lempstack%20Files/lemp%20nginx%20ctl.PNG)

To access our server locally run:

```
curl http://localhost:80
or
curl http://127.0.0.1:80
```

Now it is time for us to test how our Nginx server can respond to requests from the Internet.

Open a web browser of your choice and try to access following url

```
http://<Public-IP-Address>:80
```

![](https://github.com/Adedoja/LempStack-Project-Implementation/blob/main/Lempstack%20Files/lemp%20nginx%20webpage.PNG)

## STEP 3 -->  INSTALLING MYSQL
To install MySQL, run the command below:

```
sudo apt install mysql-server
```

When prompted, confirm installation by typing Y which means yes and then ENTER. Once the installation is finished,

log in to the MySQL console and type:

```
sudo mysql
```

This will take you to the MySQL server database. Set up your password using the command below:

```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'yourpassword';
```

To exit, run:

```
exit
```

Then run:

```
sudo mysql_secure_installation
```

This will ask if you want to configure the VALIDATE PASSWORD PLUGIN. Answer Y for yes, or anything else to continue without enabling.

When you’re finished, test if you’re able to log in to the MySQL console by typing:

```
sudo mysql -p
```

![](https://github.com/Adedoja/LempStack-Project-Implementation/blob/main/Lempstack%20Files/lemp%20mysql%20new.PNG)

Notice the -p flag in this command, which will prompt you for the password used after changing the root user password.

To exit the MySQL console, type:

```
mysql> exit
```

Your MySQL server is now installed and secured. Next, we will install PHP, the final component in the LEMP stack.

## STEP 4 – INSTALLING PHP

You have Nginx installed to serve your content and MySQL installed to store and manage your data. Now you can install PHP to process code and generate dynamic content for the web server.

While Apache embeds the PHP interpreter in each request, Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server.

To install these 2 packages at once, run:

```
sudo apt install php-fpm php-mysql
```

When prompted, type Y and press ENTER to confirm installation.
You now have your PHP components installed. Next, you will configure Nginx to use them.

Run ``` php -version ``` to confirm if installed.

![](https://github.com/Adedoja/LempStack-Project-Implementation/blob/main/Lempstack%20Files/lemp%20php.PNG)

## STEP 5 — CONFIGURING NGINX TO USE PHP PROCESSOR
When using the Nginx web server, we can create server blocks (similar to virtual hosts in Apache) to encapsulate configuration details and host more than one domain on a single server. In this guide, we will use project LEMP as an example domain name.

Create the root web directory for your domain as follows:

```
sudo mkdir /var/www/projectLEMP
```

Then, open a new configuration file in Nginx’s sites-available directory using your preferred command-line editor. Here, we’ll use vim:

```
sudo vim /etc/nginx/sites-available/projectLEMP
```

This will create a new blank file. Paste in the following bare-bones configuration:

```
#/etc/nginx/sites-available/projectLEMP
 
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
    	fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
 	}
 
	location ~ /\.ht {
    	deny all;
	}
 
}
```
When you’re done editing, save and close the file using vim commands :wq

Activate your configuration by linking to the config file from Nginx’s sites-enabled directory:

```
sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
```

This will tell Nginx to use the configuration next time it is reloaded. You can test your configuration for syntax errors by typing:

```
sudo nginx -t
```

![](https://github.com/Adedoja/LempStack-Project-Implementation/blob/main/Lempstack%20Files/lemp%20config%20ok.PNG)

You shall see following message:

```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

If any errors are reported, go back to your configuration file to review its contents before continuing.
We also need to disable default Nginx host that is currently configured to listen on port 80, for this run:

```
sudo unlink /etc/nginx/sites-enabled/default
```

Your new website is now active, but the web root /var/www/projectLEMP is still empty. Create an index.html file in that location so that we can test that your new server block works as expected:

```
sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html
```

Now go to your browser and try to open your website URL using IP address:

```
http://<Public-IP-Address>:80
```

![](https://github.com/Adedoja/LempStack-Project-Implementation/blob/main/Lempstack%20Files/lemp%20webpage%20php-nginx.PNG)

Your LEMP stack is now fully configured. In the next step, we’ll create a PHP script to test that Nginx is in fact able to handle .php files within your newly configured website.


## STEP 6 –-> TESTING PHP WITH NGINX
You can do this by creating a test PHP file in your document root. Open a new file called info.php within your document root in your text editor:

```
sudo vim /var/www/projectLEMP/info.php
```

Type or paste the following lines into the new file. This is valid PHP code that will return information about your server:

```
<?php
phpinfo();
```

You can now access this page in your web browser by visiting the domain name or public IP address you’ve set up in your Nginx configuration file, followed by /info.php:

```
http://`server_domain_or_IP`/info.php
```
You will see a web page containing detailed information about your server.

![](https://github.com/Adedoja/LempStack-Project-Implementation/blob/main/Lempstack%20Files/lemp%20php%20webpage.PNG)

## STEP 7 – RETRIEVING DATA FROM MYSQL DATABASE WITH PHP (CONTINUED)
In this step you will create a test database (DB) with simple "To do list" and configure access to it, so the Nginx website would be able to query data from the DB and display it.

We will create a database named example_database and a user named example_user, but you can replace these names with different values.

First, connect to the MySQL console using the root account:

```
sudo mysql
```
To create a new database, run the following command from your MySQL console:

```
mysql> CREATE DATABASE `example_database`;
```
Now you can create a new user and grant him full privileges on the database you have just created.

```
mysql> CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';
```
Now we need to give this user permission over the example_database database:

```
mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';
```

Now exit the MySQL shell with:

```
mysql> exit
```

You can test if the new user has the proper permissions by logging in to the MySQL console again, this time using the custom user credentials:

```
mysql -u example_user -p
```

After logging in to the MySQL console, confirm that you have access to the example_database database:

```
mysql> SHOW DATABASES;
```
This will give you the following output:

![](https://github.com/Adedoja/LempStack-Project-Implementation/blob/main/Lempstack%20Files/lemp%20database.PNG)

Next, we’ll create a test table named todo_list. From the MySQL console, run the following statement:

```
CREATE TABLE example_database.todo_list (
mysql> 	item_id INT AUTO_INCREMENT,
mysql> 	content VARCHAR(255),
mysql> 	PRIMARY KEY(item_id)
mysql> );
```

Insert a few rows of content in the test table. You might want to repeat the next command a few times, using different VALUES:

```
mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");
```

To confirm that the data was successfully saved to your table, run:

```
mysql> SELECT * FROM example_database.todo_list;
```

You’ll see the following output:
![](https://github.com/Adedoja/LempStack-Project-Implementation/blob/main/Lempstack%20Files/lemp%20database.PNG)

After confirming that you have valid data in your test table, you can exit the MySQL console:

```
mysql> exit
```

Now you can create a PHP script that will connect to MySQL and query for your content. Create a new PHP file in your custom web root directory using your preferred editor. We’ll use vi for that:

```
nano /var/www/projectLEMP/todo_list.php
```

Copy this content into your todo_list.php script:

```
<?php
$user = "example_user";
$password = "password";
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
```

Save and close the file when you are done editing.

You can now access this page in your web browser by visiting the domain name or public IP address configured for your website, followed by /todo_list.php:

```
http://<Public_domain_or_IP>/todo_list.php
```
You should see a page like this, showing the content you’ve inserted in your test table:
That means your PHP environment is ready to connect and interact with your MySQL server.

So, yeah, that's how to implement a Lemp Stack 






























































































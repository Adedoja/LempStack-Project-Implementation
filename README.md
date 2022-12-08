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

![](https://github.com/Adedoja/LempStack-Project-Implementation/blob/main/Lempstack%20Files/lemp%20mysql%20new.PNG)

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

If you answer “yes”, you’ll be asked to select a level of password validation. Keep in mind that if you enter 2 for the strongest level, you will receive errors when attempting to set any password which does not contain numbers, upper and lowercase letters, and special characters, or which is based on common dictionary words e.g PassWord.1.

When you’re finished, test if you’re able to log in to the MySQL console by typing:

```
sudo mysql -p
```

Notice the -p flag in this command, which will prompt you for the password used after changing the root user password.

To exit the MySQL console, type:

```
mysql> exit
```

Your MySQL server is now installed and secured. Next, we will install PHP, the final component in the LEMP stack.

## STEP 3 – INSTALLING PHP

You have Nginx installed to serve your content and MySQL installed to store and manage your data. Now you can install PHP to process code and generate dynamic content for the web server.

While Apache embeds the PHP interpreter in each request, Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server.

To install these 2 packages at once, run:

```
sudo apt install php-fpm php-mysql
```

When prompted, type Y and press ENTER to confirm installation.
You now have your PHP components installed. Next, you will configure Nginx to use them






















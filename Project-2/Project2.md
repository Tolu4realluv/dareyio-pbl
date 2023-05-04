# WEB STACK IMPLEMENTATION (LEMP STACK) IN AWS
---

## What is a Technology stack?
---
A stack is an arrangement of “things” kept in order one over the other. A technology stack is a set of technologies that are stacked together to build any application. Popularly known as a technology infrastructure or solutions stack, technology stack has become essential for building easy-to-maintain, scalable web applications.

Technology stack determines the type of applications you can build, the level of customizations you can perform, and the resources you need to develop your application.

For example, a web tech stack typically looks like:

![web stack](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project-1/web%20stack.png)

There are acronymns for individual technologies used together for a specific technology product. some examples are…
* LAMP (Linux, Apache, MySQL, PHP or Python, or Perl)
* LEMP (Linux, Nginx, MySQL, PHP or Python, or Perl)
* MERN (MongoDB, ExpressJS, ReactJS, NodeJS)
* MEAN (MongoDB, ExpressJS, AngularJS, NodeJS

And for this project, we are implemenitng LEMP stack solution.

---
## Connectint to the EC2 server through SSH
> NB: For this project, i already created an EC2 server on AWS, i just need to connect to it to start the implementation.


```ssh -i "mykey.pem" ubuntu@ec2-54-204-72-76.compute-1.amazonaws.com ```

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project-2/ubuntu.JPG)

Then Update my software packages by running:

```sudo apt update ```

---

## INSTALLING THE NGINX WEB SERVER
NGINX is open-source web server software used for reverse proxy, load balancing, and caching. It provides HTTPS server capabilities and is mainly designed for maximum performance and stability. It also functions as a proxy server for email communications protocols, such as IMAP, POP3, and SMTP.

In order to display web pages to our site visitors, we are going to employ Nginx, a high-performance web server. We’ll use the apt package manager to install this package:

```sudo apt install nginx ```

To verify Nginx is running:

```sudo netstat -tulpn | grep nginx ```

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project-2/Nginx.JPG)

---

## INSTALLING MYSQL

Now that we have a web server up and running, we need to install a Database Management System (DBMS) to be able to store and manage data for your site in a relational database.
MySQL is a relational database management system (RDBMS) developed by Oracle that is based on structured query language (SQL).  MySQL is a popular relational database management system used within PHP environments, so we will use it in our project.

#### To install MYSQL using ubuntu's package manager we run the following commands

```sudo apt install mysql-server```

when prompted, confirm installation by typing y

connect to the database:

```sudo mysql```

It’s recommended that we run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to our database system. Before running the script we will set a password for the root user, using mysql_native_password as default authentication method. We’re defining this user’s password as PassWord.2:

``` ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1'; ```

Then we Exit MYSQL shell:

``` exit ```

Then Start the interactive script by running:

``` sudo mysql_secure_installation ```

This asked if we'll like to set-up VALIDATE PASSWORD COMPONENT which we say NO

we can now log-in to MYSQL with the root password and run some queries:

``` sudo mysql -p ```

After successfully installing and testing mysql:

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project-2/mysql.JPG)

---

## Installing PHP

We have Nginx installed to serve our content and MySQL installed to store and manage our data. Now you can install PHP to process code and generate dynamic content for the web server.

While Apache embeds the PHP interpreter in each request, Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server. This allows for a better overall performance in most PHP-based websites, but it requires additional configuration. we’ll need to install php-fpm, which stands for “PHP fastCGI process manager”, and tell Nginx to pass PHP requests to this software for processing. Additionally, we’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies.

To install these packages at once, we run:

``` sudo apt install php php-fpm php-mysql ```

we enter 'Y' for any prompt

To check PHP is properly installed:

``` php -v ```

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project-2/php.JPG)

---

## CONFIGURING NGINX TO USE PHP PROCESSOR

When using the Nginx web server, we can create server blocks (similar to virtual hosts in Apache) to encapsulate configuration details and host more than one domain on a single server. In this guide, we will use projectLEMP as an example domain name.

On Ubuntu 20.04, Nginx has one server block enabled by default and is configured to serve documents out of a directory at /var/www/html. While this works well for a single site, it can become difficult to manage if we are hosting multiple sites. Instead of modifying /var/www/html, we’ll create a directory structure within /var/www for the our_domain website, leaving /var/www/html in place as the default directory to be served if a client request does not match any other sites.

we Create the root web directory for our_domain as follows:

``` sudo mkdir /var/www/projectLEMP ```

Next, we assign ownership of the directory with the $USER environment variable, which will reference your current system user:

we know our current username by using the command;

``` whoami ```

```sudo chown -R ubuntu:ubuntu /var/www/projectLEMP ```

Then, we open a new configuration file in Nginx’s sites-available directory using our preferred command-line editor. Here, we’ll use nano:

```sudo nano /etc/nginx/sites-available/projectLEMP ```

This will create a new blank file. then we Paste in the following bare-bones configuration:
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

Here’s what each of these directives and location blocks do:

* listen — Defines what port Nginx will listen on. In this case, it will listen on port 80, the default port for HTTP
* root — Defines the document root where the files served by this website are stored
* index — Defines in which order Nginx will prioritize index files for this website. It is a common practice to list index.html files with a higher precedence than index.php files to allow for quickly setting up a maintenance landing page in PHP applications. You can adjust these settings to better suit your application needs.
* server_name — Defines which domain names and/or IP addresses this server block should respond for. Point this directive to your server’s domain name or public IP address.
* location / — The first location block includes a try_files directive, which checks for the existence of files or directories matching a URI request. If Nginx cannot find the appropriate resource, it will return a 404 error.
* location ~ \.php$ — This location block handles the actual PHP processing by pointing Nginx to the fastcgi-php.conf configuration file and the php7.4-fpm.sock file, which declares what socket is associated with php-fpm.
* location ~ /\.ht — The last location block deals with .htaccess files, which Nginx does not process. By adding the deny all directive, if any .htaccess files happen to find their way into the document root ,they will not be served to visitors.

After completion, we save and close the file. by typing CTRL+X and then y and ENTER to confirm.

we then activate our configuration by linking to the config file from Nginx’s sites-enabled directory:

```sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/ ```

This will tell Nginx to use the configuration next time it is reloaded. we test our configuration for syntax errors by typing:

```sudo nginx -t ```


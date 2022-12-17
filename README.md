**## PROJECT-2: LEMP STACK IMPLEMENTATION**

***STEP 1: INSTALLING THE NGINX WEB SERVER***

- In order to display my web pages to visitors visiting the web, a high performance web server is needed. **NGINX** is that web server.


- I started off by updating my server package index using the command code `sudo apt update`.

- Immediately after this, I installed **NGINX** using the command code `sudo apt install nginx`.

- To varify if the **NGINX** was successfully installed and running as a service in ubuntu, I ran the command `sudo systemctl status nginx`.

<img width="925" alt="nginx status" src="https://user-images.githubusercontent.com/115954100/208251078-0f36f4b8-a6e2-4825-88a7-cc00408cad7e.PNG">

- I added a rule to EC2 configuration to open inbound connection through port 80: This allows the web server to receive traffic whenever the web page is access.

<img width="836" alt="Configuring Inbound Port 80" src="https://user-images.githubusercontent.com/115954100/208251101-001bf997-9177-4f4d-bdc0-8fb8db316336.PNG">

- With my **NGINX** server running, I checked if it can be access locally in my Ubuntu shell, using **DNS and IP address** by running the command code `curl http://localhost:80` or `curl http://127.0.0.1:80`. Which works.

<img width="612" alt="accessing my server locally on my ubuntu shell" src="https://user-images.githubusercontent.com/115954100/208251131-87264bf2-8407-4d04-950b-fd2212ff36f4.PNG">

- Finally, I tested how my Nginx server can respond to requests from the Internet. I opened a web server and entered my **public ip address** `http://44.201.87.248`.

<img width="682" alt="nginx on url" src="https://user-images.githubusercontent.com/115954100/208251162-3eee431c-58de-4848-b0b8-5d2492dfd55b.PNG">


***STEP 2: INSTALLING MYSQL***

- With my web server up and running. For data storage and management for my web server, a DBMS is needed. So I installed MYSQL for this purpose using the code `sudo apt install mysql-server`.

<img width="948" alt="MYSQL SERVER INSTALLED" src="https://user-images.githubusercontent.com/115954100/208251284-4d5d8ace-5880-4c0b-b6da-13c04d3dd326.PNG">

- Then I logged on my MYSQL Console using the command code `sudo mysql`

<img width="547" alt="Login to MYSQL Console" src="https://user-images.githubusercontent.com/115954100/208251320-83e83d91-e39a-48d5-aac8-c029e4e576e2.PNG">

- A security script that comes pre-installed with MySQL. This script removed some insecure default settings and lock down access to your database system. But before running the script I set a password for the root user, using *mysql_native_password* as default authentication method. *PassWord.1* was used as a defined user's password.

- I set a password for the root user using the command code `ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';` with 'Password.1' as my default password.

<img width="670" alt="MYSQL native password" src="https://user-images.githubusercontent.com/115954100/208251388-a41e8820-6aa7-4749-85fd-dcd351314b30.PNG">

- Using the command code `exit`, I exited the mysql shell

<img width="268" alt="MYSQL Shell exited" src="https://user-images.githubusercontent.com/115954100/208251453-56956f22-f568-4ff2-9448-2ff78824588e.PNG">

- I ran the interective script using the command code `sudo mysql_secure_installation`. This command enables me to configure and validate a new password plugin for mysql root user.

<img width="571" alt="Interactive script running and password validation" src="https://user-images.githubusercontent.com/115954100/208251609-49ac4996-acc4-463a-96e1-b4d9ad402be7.PNG">

- Then I set up a strong password

<img width="777" alt="Good password strength" src="https://user-images.githubusercontent.com/115954100/208251659-64718cff-dbd1-4580-99f3-8d840b8e91a5.PNG">

- Then i set up new rules

<img width="717" alt="Setting new rules for MYSQL" src="https://user-images.githubusercontent.com/115954100/208251686-faa0c123-0ed5-4e3d-8247-1f0301c0d465.PNG">

- Then I ran a command code to check if I can log in to MYSQL Console with the new password using the command code `sudo mysql -p`.

<img width="682" alt="New password login to MYSQL Console" src="https://user-images.githubusercontent.com/115954100/208251750-9c893777-8f54-4b90-8102-f7925645d157.PNG">

- Finally, I exited my MYSQL Console using the command code `exit`


***STEP 3: INSTALLING PHP***

- Now I have Nginx installed to serve my content and MySQL installed to store and manage my data. I then installed PHP to process code and generate dynamic content for the web server.

- Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server. This allows for a better overall performance in most PHP-based websites, but it requires additional configuration. You’ll need to install **php-fpm**, which stands for “PHP fastCGI process manager”, and tell Nginx to pass PHP requests to this software for processing. Additionally, you’ll need **php-mysql**, a PHP module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies.

- With the command code `sudo apt install php-fpm php-mysql` these packages **PHP-FPM and PHP-MYSQL** were intalled simultaneously.

<img width="892" alt="PHP Components installed" src="https://user-images.githubusercontent.com/115954100/208252885-ab390371-f3f1-495b-baf9-aeed6ca0f3d1.PNG">


***STEP 4: CONFIGURING NGINX TO USE PHP PROCESSOR***

- With **Nginx web server**, we can create server blocks (similar to virtual hosts in Apache) to encapsulate configuration details and host more than one domain on a single server. In this guide, we will use *projectLEMP* as an example domain name.

- On Ubuntu 20.04, Nginx has one server block enabled by default and is configured to serve documents out of a directory at **/var/www/html**. While this works well for a single site, it can become difficult to manage if you are hosting multiple sites. Instead of modifying **/var/www/html**, we’ll create a directory structure within **/var/www** for the your_domain website, leaving **/var/www/html** in place as the default directory to be served if a client request does not match any other sites.

- I created a root web directory for a domain. Let this domain be called **projectLEMP**. So I use the command code `sudo mkdir /var/www/projectLEMP`

<img width="470" alt="Created root web directory" src="https://user-images.githubusercontent.com/115954100/208253026-cc17cfb5-213d-42b9-bc67-55d814ea55a7.PNG">

- Using the command code `sudo chown -R $USER:$USER /var/www/projectLEMP`, i assigned ownership of the directory with the $USER environment variable, which will reference the  current system user

<img width="573" alt="Assigned ownership of the directory" src="https://user-images.githubusercontent.com/115954100/208253050-e532fb9b-35ca-4605-bfea-6a0b3a3f9e42.PNG">

- Then, I opened a new configuration file in *Nginx’s sites-available directory* using *nano* command-line editor, I entered the code `sudo nano /etc/nginx/sites-available/projectLEMP`

- With this, a new blank file was created and the below *bare bones file* was pasted in the blank file. 

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
- Here’s what each of these directives and location blocks do:

```

- listen — Defines what port Nginx will listen on. In this case, it will listen on port 80, the default port for HTTP.
- root — Defines the document root where the files served by this website are stored.
- index — Defines in which order Nginx will prioritize index files for this website. It is a common practice to list index.html files with a higher precedence than index.php files to allow for quickly setting up a maintenance landing page in PHP applications. You can adjust these settings to better suit your application needs.
- server_name — Defines which domain names and/or IP addresses this server block should respond for. Point this directive to your server’s domain name or public IP address.
- location / — The first location block includes a try_files directive, which checks for the existence of files or directories matching a URI request. If Nginx cannot find the appropriate resource, it will return a 404 error.
- location ~ \.php$ — This location block handles the actual PHP processing by pointing Nginx to the fastcgi-php.conf configuration file and the php7.4-fpm.sock file, which declares what socket is associated with php-fpm.
- location ~ /\.ht — The last location block deals with .htaccess files, which Nginx does not process. By adding the deny all directive, if any .htaccess files happen to find their way into the document root ,they will not be served to visitors.
```

- After editing, I saved and closed the file. Since nano editor was used, I saved by typing CTRL+X and then y and ENTER to confirm.

- My configuration was activated by linking it to the config file from *Nginx’s sites-enabled directory* using the command code `sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/`. This instructed Nginx to use the configuration next time it is reloaded. So I tested my configuration for syntax errors by typing the code `sudo nginx -t`

- With the output showing successful

<img width="788" alt="No syntax error in my configuration" src="https://user-images.githubusercontent.com/115954100/208253363-2e114f99-e6ad-44ec-a4c3-df490572bba1.PNG">

- I disabled the default Nginx host that is currently configured to listen on port 80, running the command code `sudo unlink /etc/nginx/sites-enabled/default`

- To apply the changes made, I reloaded *Nginx* using the command code `sudo systemctl reload nginx`

- My new website is now active, but the web root */var/www/projectLEMP* is still empty. I created an *index.html* file in that location so I can test to confirm that my new server block works as expected

- `sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html`

- I went to my browser and opened my website URL using IP address: *http://<Public-IP-Address>:80*

-I got an *‘echo’* command I wrote to my *index.html* file, meaning my *Nginx* site is working as expected

<img width="591" alt="website url using my public IP address" src="https://user-images.githubusercontent.com/115954100/208253552-eb19c8f6-ea52-4e95-9b6d-eb4c463592a4.PNG">


***STEP 5: TESTING PHP WITH NGINX***

- With my *LAMP stack* complately installed and fully operational.

- I tested the *LAMP stack* to validate that Nginx can correctly hand *.php* files off to my PHP processor.

- I created a test PHP file in your document root. I opened a new file called *info.php* within the document root in my text editor by using the command code `sudo nano /var/www/projectLEMP/info.php`

- I typed or pasted the following lines into the new file. This is valid PHP code that will return information about my server. 

```
<?php
phpinfo();
```
- Entering my *public IP/info.php. address on my web brower* `http://`server_domain_or_IP`/info.php`

<img width="839" alt="Accessing my web page" src="https://user-images.githubusercontent.com/115954100/208253831-9c929adb-c849-40fd-bcfa-2853fb580053.PNG">

- so a web page containing detailed information about my server

- So I removed the file I created as it contains sensitive information about my PHP environment and my Ubuntu server. i used *rm* to remove that file: So I ran the command code `sudo rm /var/www/your_domain/info.php`

<img width="536" alt="removing domain file" src="https://user-images.githubusercontent.com/115954100/208254013-28a38a81-bf08-4f9a-b59a-c676ce08d03a.PNG">


***STEP 6: RETRIEVING DATA FROM MYSQL DATABASE WITH PHP***

- In this step I created a test database (DB) with simple "To do list" and configured access to it, so the Nginx website would be able to query data from the DB and display it.

- Presently, the native MySQL PHP library *mysqlnd* doesn’t support *caching_sha2_authentication*, the default authentication method for *MySQL 8*.So I created a new user with the *mysql_native_password* authentication method in order to be able to connect to the MySQL database from PHP.
- I created a database named **example_database** and a user named **example_user**, but you can replace these names with different values of your choice.

- I connected to MySQL Console using the root account `sudo mysql`

<img width="555" alt="Logged in to mysql console" src="https://user-images.githubusercontent.com/115954100/208254153-36e0e7cf-20dc-4dec-8e84-a031cc1492f1.png">

- I created a new database, by running the following command from my MySQL console `mysql> CREATE DATABASE *example_database*;`

<img width="380" alt="Creating a new database" src="https://user-images.githubusercontent.com/115954100/208254304-ed877715-e2ce-4864-ba1f-c85d66324805.png">

- After creating this database, I created a new user and granted the new user full privileges

- Using *mysql_native_password* as default authentication method, I created a new user named *example_user* and I defined a password for *example_user* as *@password1*. All these is done using the command code `mysql>  CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY '*@password1';`

<img width="560" alt="using mysql native password to create new user name" src="https://user-images.githubusercontent.com/115954100/208254413-3da6801e-39fd-4bb6-aa11-d2115a39ee4a.png">

- After this, I granted the *user* permission over the *example_database* database using the command code `mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';`

<img width="452" alt="Granting permission to user" src="https://user-images.githubusercontent.com/115954100/208254643-a19bead9-14ea-4fd8-a12e-0071bd5c3d63.png">

- Now the user *example_user* has full permission over *example_database* database, while preventing this user from creating or modifying other databases on my server.

- After this, I exited *MySQL* shell using the command code `mysql> exit`

- I tested if the new user has the proper permissions by logging in to the *MySQL console* again, this time using the custom user credentials. So I used the command code `mysql -u example_user -p`. **Note**, the password entered is *@password1*

<img width="566" alt="exit and loggin into the console using the new custom password" src="https://user-images.githubusercontent.com/115954100/208254710-97bcf628-77c3-41fc-828e-b1005624cd9d.png">

- After logging in to my MySQL console, I confirmed that I have access to the *example_database* database using the command code `mysql> SHOW DATABASES;`

<img width="236" alt="Database output" src="https://user-images.githubusercontent.com/115954100/208254769-280c37c8-19c9-4082-ac0e-2170c664edcb.png">

- Next, I created a test table named **todo_list**. From the MySQL console, I ran the following statement 
`CREATE TABLE example_database.todo_list ( item_id INT AUTO_INCREMENT, content VARCHAR(255), PRIMARY KEY(item_id));`

- I Inserted a few rows of content in the test table. I repeated the next command a few times, using different VALUES:

- `mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");`

- To confirm that the data was successfully saved to my table, I ran the command code

- `mysql>  SELECT * FROM example_database.todo_list;`

- I see the following output:

<img width="677" alt="Data successfully saved to table with the output" src="https://user-images.githubusercontent.com/115954100/208255608-1b59d64b-46a5-4812-8932-58c0b963b7c4.png">

- Afterwish I exited the console using the command code `mysql> exit`

- I created a PHP script that will connect to MySQL and query my content. I also created a new PHP file in my custom web root directory using my preferred editor. I used vi for this, running the code `nano /var/www/projectLEMP/todo_list.php`

- The following PHP script connects to the MySQL database and queries for the content of the **todo_list** table, displays the results in a list. If there is a problem with the database connection, it will throw an exception.

- I Copied this content into my *todo_list.php* script

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
- **Note** mysql:host=*projectLEMP*

- using vi editor, I saved and closed.

- I accessed this page in my web browser by visiting the domain name or public IP address configured for my website, followed by */todo_list.php*:

- `http://<Public_domain_or_IP>/todo_list.php`

- I got the successful feedback.

<img width="462" alt="PHP environment ready to interact with MQL Server" src="https://user-images.githubusercontent.com/115954100/208255836-bfa44561-2fdd-4922-8bba-bc506962018f.png">


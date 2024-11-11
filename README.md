# LAMP Stack Implementation AWS
In this project, I demonstrate the implementation of a LAMP stack project in AWS.                                                        

LAMP Stack is one of the various technology stacks that exist. A technology stack is a set of frameworks and tools used to develop a software product. This set of frameworks and tools are very specifically chosen to work together in creating a well-functioning software. They are acronymns for individual technologies used together for a specific technology product. LAMP (Linux, Apache, MySQL, PHP or Python, or Perl) is one of them. Others include; LEMP (Linux, Nginx, MySQL, PHP or Python, or Perl), MERN (MongoDB, ExpressJS, ReactJS, NodeJS), MEAN (MongoDB, ExpressJS, AngularJS, NodeJS)

To perform this project, I must carry out the following steps
- Create a EC2 Instance
- Install APACHE and Update the Firewall
- Install MYSQL
- Install PHP
- Create Virtual Host Using APACHE
- Enable PHP on the website

## Create a EC2 Instance
In order to complete this project you will need an AWS account and a virtual server with Ubuntu Server OS.

AWS is the biggest Cloud Service Provider and it offers a free tier account that we are going to leverage for our projects.

Follow the instructions below to create your EC2 instance.

1. Register a new AWS account.
2. Select your preferred region (the closest to you) and launch a new EC2 instance of t2.micro family with Ubuntu Server launch EC2
   
IMPORTANT – save your private key (.pem file) securely and do not share it with anyone! If you lose it, you will not be able to connect to your server ever again!
### Connecting to EC2 terminal
- Move into the folder where the pair key is downloaded and run the following command to connect to the instance
```
      cd ~/Downloads
      sudo chmod 0400 <private-key-name>.pem
      ssh -i <private-key-name>.pem ubuntu@<Public-IP-address>
```
## Install APACHE and Update the Firewall
1. Install Apache using Ubuntu’s package manager ‘apt’
```
      #update a list of packages in package manager
      sudo apt update

      #run apache2 package installation
      sudo apt install apache2 
```
2. Verify that apache2 is running as a Service in our OS, using the following command
```
      sudo systemctl status apache2
```
3. I tested if I can access the server locally by running the following command
```
      curl http://localhost:80
      or
      curl http://127.0.0.1:80
```
4.  Open a web browser of your choice and try to access following url
```
      http://<Public-IP-Address>:80
```
## Install MYSQL
1. Using ‘apt’ to acquire and install this server
```
      $ sudo apt install mysql-server
```
2. log in to the MySQL console by typing
```
      $ sudo mysql
```
3. Exit the MySQL shell with:
```
      mysql> exit
```
4. Start the interactive script by running:
```
      sudo mysql_secure_installation
```
This will ask if you want to configure the VALIDATE PASSWORD PLUGIN.

Note: Enabling this feature is something of a judgment call. If enabled, passwords which don’t match the specified criteria will be rejected by MySQL with an error. It is safe to leave validation disabled, but you should always use strong, unique passwords for database credentials.

Answer Y for yes, or anything else to continue without enabling.

## Install PHP
1. I will install the php package, in addition to the php package, I’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases and also libapache2-mod-php to enable Apache to handle PHP files
```
      sudo apt install php libapache2-mod-php php-mysql
```
2. Once the installation is finished, you can run the following command to confirm your PHP version:
```
      php -v
```
## Create Virtual Host Using APACHE
1. Create the directory for projectlamp using ‘mkdir’ command as follows:
```
      sudo mkdir /var/www/projectlamp
```
2. Next, assign ownership of the directory with your current system user:
```
      sudo chown -R $USER:$USER /var/www/projectlamp
```
3.  Create and open a new configuration file in Apache’s sites-available directory using your preferred command-line editor.
```
      sudo vi /etc/apache2/sites-available/projectlamp.conf
```
4. paste the command below in the vim file and save
```
       <VirtualHost *:80>
      ServerName projectlamp
      ServerAlias www.projectlamp 
      ServerAdmin webmaster@localhost
      DocumentRoot /var/www/projectlamp
      ErrorLog ${APACHE_LOG_DIR}/error.log
      CustomLog ${APACHE_LOG_DIR}/access.log combined
      </VirtualHost>
```
5. You can now use a2ensite command to enable the new virtual host:
```
      sudo a2ensite projectlamp
```
6. You can disable Apache default website using the command
```
      sudo a2dissite 000-default
```
7. To make sure your configuration file doesn’t contain syntax errors, run:
```
      sudo apache2ctl configtest
```
8. Finally, reload Apache so these changes take effect:
```
      sudo systemctl reload apache2
```
9. Now go to your browser and try to open your website URL using IP address:
```
      http://<Public-IP-Address>:80
```
## Enable PHP on the website
1. Edit the conf file with the command
```
      sudo vim /etc/apache2/mods-enabled/dir.conf
```
2.  #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
    #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm

3. After saving and closing the file, you will need to reload Apache so the changes take effect:
```
      sudo systemctl reload apache2
```
4. Create a new file named index.php inside your custom web root folder:
```
      vim /var/www/projectlamp/index.php
```
5. This will open a blank file. Add the following text, which is valid PHP code, inside the file:
```
      <?php
      phpinfo();
```
6. When you are finished, save and close the file, refresh the page and you will see a page similar to this:

7.After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment -and your Ubuntu server. You can use rm to do so:
```
      sudo rm /var/www/projectlamp/index.php
```



    

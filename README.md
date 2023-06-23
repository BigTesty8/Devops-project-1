# Devops-project-1
## WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS
Firstly,create a security group and ensure to choose the right inbound/outbound settings which consist of the rule and source
you would need to select the ALL TRAFFIC and ANYWHERE suggestion, ADD NEWTAG should be security name and save successfully.
- launched an instance
- select connect and copy the SSH with example > go to terminal > cd Downloads > paste the the ssh and instance will be installed succesfully.
  
## STEP 1 — INSTALLING APACHE AND UPDATING THE FIREWALL
#update a list of packages in package manager
sudo apt update

#run apache2 package installation
sudo apt install apache2
copy IP address created from instance created and paste on google here make sure security group is selected.
If you see following page, then your web server is now correctly installed and accessible through your firewall.
![Screenshot 2023-06-23 084037](https://github.com/BigTesty8/Devops-project-1/assets/137091610/bc789372-8614-4346-844d-241838217f83)
## STEP 2 — INSTALLING MYSQL
$ sudo apt install mysql-server
$ sudo mysql
Too change passwrd of root user > ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
Flush Priveleges , ; and exit and bye > $ sudo mysql_secure_installation.
VALIDATE PASSWORD PLUGIN can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD plugin?

Press y|Y for Yes, any other key for No:
When you’re finished, test if you’re able to log in to the MySQL console by typing:

$ sudo mysql -p
To exit the MySQL console, type:

mysql> exit

Notice that you need to provide a password to connect as the root user.
![Screenshot 2023-06-23 090640](https://github.com/BigTesty8/Devops-project-1/assets/137091610/93367370-fd0c-45aa-91fb-f25cfd003d0a)

## STEP 3 — INSTALLING PHP
To install these 3 packages at once, run:

sudo apt install php libapache2-mod-php php-mysql
Once the installation is finished, you can run the following command to confirm your PHP version:

php -v
PHP 7.4.3 (cli) (built: Oct  6 2020 15:47:56) ( NTS )
Copyright (c) The PHP Group
Zend Engine v3.4.0, Copyright (c) Zend Technologies
![Screenshot 2023-06-23 093306](https://github.com/BigTesty8/Devops-project-1/assets/137091610/19b5eef7-c78f-46ff-bae5-593955dc5eb0)

## STEP 4 — CREATING A VIRTUAL HOST FOR YOUR WEBSITE USING APACHE
sudo mkdir /var/www/projectlamp
Next, assign ownership of the directory with your current system user:

 sudo chown -R $USER:$USER /var/www/projectlamp
 Then, create and open a new configuration file in Apache’s sites-available directory using your preferred command-line editor. Here, we’ll be using vi or vim (They are the same by the way):

sudo vi /etc/apache2/sites-available/projectlamp.conf
This will create a new blank file. Paste in the following bare-bones configuration by hitting on i on the keyboard to enter the insert mode, and paste the text:

<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
To save and close the file, simply follow the steps below:

Hit the esc button on the keyboard
Type :
Type wq. w for write and q for quit
Hit ENTER to save the file
You can use the ls command to show the new file in the sites-available directory

sudo ls /etc/apache2/sites-available
You will see something like this;

000-default.conf  default-ssl.conf  projectlamp.conf
With this VirtualHost configuration, we’re telling Apache to serve projectlamp using /var/www/projectlampl as its web root directory. If you would like to test Apache without a domain name, you can remove or comment out the options ServerName and ServerAlias by adding a # character in the beginning of each option’s lines. Adding the # character there will tell the program to skip processing the instructions on those lines.
You can now use a2ensite command to enable the new virtual host:

sudo a2ensite projectlamp
You might want to disable the default website that comes installed with Apache. This is required if you’re not using a custom domain name, because in this case Apache’s default configuration would overwrite your virtual host. To disable Apache’s default website use a2dissite command , type:

sudo a2dissite 000-default
To make sure your configuration file doesn’t contain syntax errors, run:

sudo apache2ctl configtest
Finally, reload Apache so these changes take effect:

sudo systemctl reload apache2
Your new website is now active, but the web root /var/www/projectlamp is still empty. Create an index.html file in that location so that we can test that the virtual host works as expected:

sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
Now go to your browser and try to open your website URL using IP address:

http://<Public-IP-Address>:80
If you see the text from ‘echo’ command you wrote to index.html file, then it means your Apache virtual host is working as expected.
In the output you will see your server’s public hostname (DNS name) and public IP address. You can also access your website in your browser by public DNS name, not only by IP – try it out, the result must be the same (port is optional)

http://<Public-DNS-Name>:80
You can leave this file in place as a temporary landing page for your application until you set up an index.php file to replace it. Once you do that, remember to remove or rename the index.html file from your document root, as it would take precedence over an index.php file by default.
## STEP 5 — ENABLE PHP ON THE WEBSITE

sudo vim /etc/apache2/mods-enabled/dir.conf
After saving and closing the file, you will need to reload Apache so the changes take effect:
sudo systemctl reload apache2
Create a new file named index.php inside your custom web root folder:   vim /var/www/projectlamp/index.ph
<?php
phpinfo();
![Screenshot 2023-06-23 101509](https://github.com/BigTesty8/Devops-project-1/assets/137091610/c5a19207-ae05-4bd6-b401-cca85881737f)


























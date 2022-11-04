## Deploying rails application with Passenger + Apache + ubuntu 20.04
Apache installation
```
sudo apt update
sudo apt install apache2
```
Install Passenger
```
gem install passenger
```
Prerequisite for apache passenger

Install libcurl for client-side URL transfer
```
sudo apt-get install libcurl4-openssl-dev
```
To install Apache 2 development headers:
```
apt-get install apache2-dev
```
To install Apache Portable Runtime (APR) development headers:
```
apt-get install libapr1-dev
```
To install Apache Portable Runtime Utility (APU) development headers:
```
apt-get install libaprutil1-dev
```
Install the apache passenger module
```
passenger-install-apache2-module
```
this will take some time. read carefully what it prompts. if you are missing any prerequisites it will prompt you. install those if any. in the end, it will ask you to add some lines to the apache config file, open a new terminal and add those lines to /etc/apache2/apache2.conf and append those lines. save and exit. restart apache
```
#add lines to this file
sudo nano  /etc/apache2/apache2.conf
#restart apache 
sudo service apache2 restart
```
Deploying an app to a virtual host’s root

create a new configuration file in the apache2 directory

```
sudo nano /etc/apache2/sites-available/testapp.conf
```
Determine the Ruby command that Passenger should use

```
passenger-config about ruby-command
```
Here is the sample template you can use for the configuration file.
```
# /etc/apache2/sites-available/testapp.conf
<VirtualHost *:80>
    ServerName yourserver.com

    # Tell Apache and Passenger where your app's 'public' directory is
    DocumentRoot /path-to-your-app/public

    PassengerRuby /path-to-ruby

    # Relax Apache security settings
    <Directory /path-to-your-app/public>
      Allow from all
      Options -MultiViews
      # Uncomment this if you're on Apache > 2.4:
      #Require all granted
    </Directory>
</VirtualHost>
```
put your server name or IP address instead of yourserver.com. and replace path-to-your-app with your rails application dir path. put path-to-ruby we found in the previous step.

Make sure the Passenger Apache module; it may be enabled already:
```
sudo a2enmod passenger
```
disable the default apache site and enable your site .once done restart Apache:
```
sudo a2dissite 000-default
sudo a2ensite testapp
sudo service apache2 restart
```

# Nagios
The Industry Standard In IT Infrastructure Monitoring


# Nagios Instalation in Ubuntu 22.04

## Prerequisites

Perform these steps to install the pre-requisite packages.

`` sudo apt-get update ``

`` sudo apt-get install -y autoconf gcc libc6 make wget unzip apache2 php libapache2-mod-php7.4 libgd-dev ``

`` sudo apt-get install openssl libssl-dev ``

## Downloading the Source

`` cd /tmp ``

`` wget -O nagioscore.tar.gz https://github.com/NagiosEnterprises/nagioscore/archive/nagios-4.4.6.tar.gz ``

`` tar xzf nagioscore.tar.gz ``

## Compile

`` cd /tmp/nagioscore-nagios-4.4.6/ ``

`` sudo ./configure --with-httpd-conf=/etc/apache2/sites-enabled ``

`` sudo make all ``

## Create User And Group

This creates the nagios user and group. The www-data user is also added to the nagios group.

`` sudo make install-groups-users ``

`` sudo usermod -a -G nagios www-data ``

## Install Binaries
This step installs the binary files, CGIs, and HTML files.

`` sudo make install ``

## Install Service / Daemon

This installs the service or daemon files and also configures them to start on boot.

`` sudo make install-daemoninit ``

Information on starting and stopping services will be explained further on.
# Install Command Mode

This installs and configures the external command file.

`` sudo make install-commandmode ``

## Install Configuration Files

This installs the *SAMPLE* configuration files. These are required as Nagios needs some configuration files to allow it to start.

`` sudo make install-config ``

## Install Apache Config Files

This installs the Apache web server configuration files and configures Apache settings.

`` sudo make install-webconf ``

`` sudo a2enmod rewrite ``

`` sudo a2enmod cgi ``

## Create nagiosadmin User Account

You’ll need to create an Apache user account to be able to log into Nagios.

The following command will create a user account called nagiosadmin and you will be prompted to provide a password for the account.

`` sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin ``

## Start Apache Web Server

This command starts Nagios Core.

`` sudo systemctl start nagios.service ``

## Test Nagios

Nagios is now running, to confirm this you need to log into the Nagios Web Interface.

Point your web browser to the ip address or FQDN of your Nagios Core server, for example:

`` http://17.33.3.233/nagios ``

# Nagios Architecture

![image](https://user-images.githubusercontent.com/42967535/220137892-48ada28e-725b-45e7-920d-3cde7043c9ba.png)

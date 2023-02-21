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

#  Install Nagios Plugins

- Download the Nagios Core plugin. To download the latest plugins, visit the plugins download page.

`` $ cd ~/ ``

`` $ wget https://nagios-plugins.org/download/nagios-plugins-2.3.3.tar.gz ``

- Extract the downloaded plugin.

`` sudo tar -zxvf nagios-plugins-2.3.3.tar.gz ``

- Navigate to the plugins' directory.

`` cd nagios-plugins-2.3.3/ ``

- Run the plugin configure script.

`` sudo ./configure --with-nagios-user=nagios --with-nagios-group=nagios ``

- Compile Nagios Core plugins.

`` sudo make ``

- nstall the plugins.

`` sudo make install ``

# Verify Nagios Configuration

- Verify the Nagios Core configuration.

`` sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg ``

- Start the Nagios service.

`` sudo systemctl start nagios ``

- Enable Nagios service to run at system startup.

`` sudo systemctl enable nagios ``

# Add Remote Hosts

Now that your Nagios server is configured, it's time to configure a remote host to monitor.

To get started, connect as root over SSH to the host you want to monitor.

### Install Prerequisites

To monitor hosts, we need to add them to Nagios. By default, Nagios only monitors localhost (the server it's running on). We're going to add hosts that are part of our network to gain even more control. You will need to use the following instructions on all hosts that you want to monitor.

First, install *nagios-plugins* and *nagios-nrpe-server*:

`` sudo apt-get install nagios-plugins nagios-nrpe-server ``

### Configure NRPE

Next, open the /etc/nagios/nrpe.cfg file. Replace the value of allowed_hosts with 127.0.0.1,0.0.0.0 replacing the second IP with the IP address of the Nagios server.

We will now open the file /etc/nagios/nrpe.cfg and replace a couple of values.


    Replace the value of server_address to the private IP address of the host.

    Set allowed_hosts to the private IP address of your Nagios server.

    Execute df -h / , copy the output, and put that as the value of command . It indicates your root file system.

Save the file when you are finished.

Now restart NRPE:

`` sudo service nagios-nrpe-server restart ``

# Add the Host to Nagios

Now that we've configured the host we're going to monitor, we need to switch back to our Nagios server and add the host to it. Open the following file with your favorite editor:

`` nano /usr/local/nagios/etc/servers/host.cfg ``

Now that we've configured the host we're going to monitor, we need to switch back to our Nagios server and add the host to it. Open the following file with your favorite editor:

`` vim /usr/local/nagios/etc/servers/host.cfg ``

```
define host {

        use                             linux-server

        host_name                       yourhost

        alias                           My first Apache server

        address                         1.2.3.4

        max_check_attempts              5

        check_period                    24x7

        notification_interval           30

        notification_period             24x7

}

```

This will allow you to simply monitor whether the server is up or down. Now reload Nagios:

`` service nagios reload ``






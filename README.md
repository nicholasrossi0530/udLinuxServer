# udLinuxServer
ssh -i ubuntu-key.pem grader@34.220.104.43 -p 2200

PASSWORD FOR GRADER: password

i. IP: 34.220.104.43 SSH: 2200
ii. http://34.220.104.43/music/1/menu
iii. List of software installed:
    finger, apache2, postgresql, libapache2-mod-wsgi, git, python-pip, flask, sqlalchemy, psycopg2
    Config changes:
    added user grader with sudo permissions, created music_catalog db in postgres with user catalog with all permissions, changed time zone to UTC, added proper ports to firewall and enabled, changed ssh to port 2200, set up grader user with Lightsail key, Amazon Lightsail, added proper ports to Lightsail firewall

    1. Amazon lightsail:
        go to the lightsail website, make an account and create an ubuntu os only instance, allow 2200 and 123 on the firewall and download the ssh key
    2. Click the ssh button on lightsail to log in to the server
    3. Update all currently installed packages
        sudo apt-get update
        sudo apt-get upgrade
    4. Change ssh port 22 to 2200
        sudo nano /etc/ssh/sshd_config 
        change port from 22 to 2200 
        set PermitRootLogin from prohibit-password to No
    5. Configure the UFW firewall
        sudo ufw allow 2200/tcp
        sudo ufw allow 80/tcp
        sudo ufw allow 123/udp
        sudo ufw default deny incoming
        sudo ufw default allow outgoing
        sudo ufw allow ntp
        sudo ufw added
        sudo ufw enable
        sudo ufw status
    6. Create a new user account named grader. +
    7. Give grader the permission to sudo. +
    8. Create an SSH key pair for grader using the ssh-keygen tool.
        sudo adduser grader
        sudo visudo
        append grader	ALL=(ALL:ALL) 
        sudo nano /etc/sudoers.d/grader. 
        Give grader the super permisssion grader ALL=(ALL:ALL) ALL
        sudo chown grader:grader /home/grader/.ssh
        sudo chmod 700 /home/grader/.ssh
        sudo cp /root/.ssh/authorized_keys /home/grader/.ssh/
        sudo chown grader:grader /home/grader/.ssh/authorized_keys
        sudo chmod 644 /home/grader/.ssh/authorized_keys        
        sudo nano /home/grader/.ssh/authorized_keys
        su - grader
        mkdir .ssh
    9. Configure the local timezone to UTC.
        sudo dpkg-reconfigure tzdata
        follow the on screen ui to configure UTC
    10. Install and configure Apache to serve a Python mod_wsgi application.
        sudo apt-get install apache2
        sudo a2enmod wsgi
        sudo service apache2 start
        sudo service apache2 restart

        sudo nano /var/www/catalog/catalog.wsgi
        #!/usr/bin/python
        import sys
        import logging
        logging.basicConfig(stream=sys.stderr)
        sys.path.insert(0, "/var/www/catalog/")

        sudo nano /etc/apache2/sites-available/catalog.conf
        <VirtualHost *:80>
            ServerName 18.188.132.132
            ServerAdmin admin@18.188.132.132
            WSGIScriptAlias / /var/www/catalog/catalog.wsgi
            <Directory /var/www/catalog/catalog/>
                Order allow,deny
                Allow from all
            </Directory>
            Alias /static /var/www/catalog/catalog/static
            <Directory /var/www/catalog/catalog/static/>
                Order allow,deny
                Allow from all
            </Directory>
            ErrorLog ${APACHE_LOG_DIR}/error.log
            LogLevel warn
            CustomLog ${APACHE_LOG_DIR}/access.log combined
        </VirtualHost>
    11. Install and configure PostgreSQL
        sudo apt-get install postgresql
        sudo -u postgres createuser -P catalog
        sudo -u postgres createdb -O music_catalog catalog
        sudo -u postgres -i
        psql
        \c catalog
        REVOKE ALL ON SCHEMA public FROM public;
        GRANT ALL ON SCHEMA public TO music_catalog;
    12. Install git.
        sudo apt-get install git
    13. Clone and setup your Item Catalog project
        git clone https://github.com/nicholasrossi0530/udItemCatalog catalog
    14.Set it up in your server so that it functions correctly
    sudo nano /etc/apache2/sites-available/catalog.conf
        <VirtualHost *:80>
            ServerName 18.188.132.132
            ServerAdmin admin@18.188.132.132
            WSGIScriptAlias / /var/www/catalog/catalog.wsgi
            <Directory /var/www/catalog/catalog/>
                Order allow,deny
                Allow from all
            </Directory>
            Alias /static /var/www/catalog/catalog/static
            <Directory /var/www/catalog/catalog/static/>
                Order allow,deny
                Allow from all
            </Directory>
            ErrorLog ${APACHE_LOG_DIR}/error.log
            LogLevel warn
            CustomLog ${APACHE_LOG_DIR}/access.log combined
        </VirtualHost>

iv. Sources used: Udacity classroom forums, Udacity class code, StackOverflow,
    Python forums, Apache2 forums, Lightsail forums

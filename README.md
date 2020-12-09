# Sogoresearch-Server-Config

First of all, connect to the VM using SSH:
```sh
$ ssh user@ip_address (ex: ssh nick@12.34.56.789)
```

Then, update the server:
```sh
$ sudo apt-get update
$ sudo apt-get upgrade
```

### Installing Python 3.6 and Pip3
```sh
sudo apt install python3-pip
```

### Installing Apache and WSGI
First, to install Apache you'll need to run:
```sh
$ sudo apt-get install apache2
```

After that, we need to get WSGI, so run the following:
```sh
$ sudo apt-get install libapache2-mod-wsgi-py3
```

Once we have that, we need to make sure we've enabled WSGI with the following:
```sh
$ sudo a2enmod wsgi
```

### Configure WSGI
#### Creating emojiset_app.conf
So now we need to set up our Flask configuration file:
```sh
$ cd /etc/apache2/sites-available/
$ sudo nano emojiset_app.conf
```
This is where your Flask configuration goes, which will apply to your live web site. Here's the code that you need to include:
```sh
<VirtualHost *:80>
                ServerName yourdomain.com(yours system ipaddress)
                ServerAdmin youemail@email.com
                WSGIScriptAlias / /var/www/html/flaskapp.wsgi
                WSGIApplicationGroup %{GLOBAL}
                WSGIProcessGroup %{GLOBAL}
                WSGIDaemonProcess emojiset_app processes=2 threads=6
                <Directory /var/www/html/EmojisetWebsite/emojiset_app/ >
                        Order allow,deny
                        Allow from all
                </Directory>
                Alias /static /var/www/html/EmojisetWebsite/emojiset_app/static
                <Directory /var/www/FlaskApp/FlaskApp/static/>
                        Order allow,deny
                        Allow from all
                </Directory>
                ErrorLog ${APACHE_LOG_DIR}/error.log
                LogLevel warn
                CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

For your notes, if you want to add more domains/subdomains that point to the same Flask App, or a different app entirely, you can use a ServerAlias, added underneath the ServerAdmin line.
We are now ready to enable the server.
Run:
```sh
$ sudo a2ensite FlaskApp
$ service apache2 reload
```
Move to the directory where our application will be located:
```sh
$ cd /var/www/html/
```
#### Creating flaskapp.wsgi
Create a wsgi config file
```sh
$ sudo nano flaskapp.wsgi
```

Within the wsgi file, enter:
```sh
#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/html/EmojisetWebsite")

from emojiset_app import app as application
application.secret_key = 'fhkjdskjgf(anything)’
```

Restart Apache:
```sh
$ sudo service apache2 restart
```

### Get code from GitHub
Clone code from GitHub:
```sh
$ sudo git clone https://github.com/username/RepositoryName.git
```
The application is now located in /var/www/html/EmojisetWebsite

### Install Python Libraries
There is a requirements.txt file in /var/www/html/EmojisetWebsite
```sh
$ cd /var/www/html/EmojisetWebsite
$ sudo pip3 install -r requirements.txt
```

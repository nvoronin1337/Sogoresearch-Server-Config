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
Move to the directory where our application will be located:
```sh
$ cd /var/www/html/
```
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

### Get home from GitHub
Clone code from GitHub:
```sh
$ sudo git clone https://github.com/username/RepositoryName.git
```
The application is now located in /var/www/html/EmojisetWebsite




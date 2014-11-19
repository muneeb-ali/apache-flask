apache-flask
============

Python Flask (web framework) deployed with Apache

### Quick Start

1. fork this repo

2. clone your own version
> git clone git@github.com:YOUR_GITHUB_USERNAME/apache-flask.git
> cd apache-flask

3. install all the necessary packages (best done inside of a virtual environment)
> pip install -r requirements.txt

4. run the app
> python runserver.py

5. check out the test page and make your modifications
> http://localhost:5000

### Deploying with Apache

These instructions are for Debian, for other Linux flavors modify the Apache parts accordingly

Make sure apache2 and mod_wsgi are installed

```
sudo apt-get install -y apache2
sudo apt-get install -y libapache2-mod-wsgi
sudo a2enmod rewrite
sudo apt-get install -y apache2-utils
```

For this examples, we assume your app is in /srv/www/app. In /etc/apache2/sites-available/000-default.conf file add:
```
    ServerName 0.0.0.0

<VirtualHost _default_:80>
    DocumentRoot /srv/www/app

    WSGIDaemonProcess python-app user=www-data group=www-data threads=15 maximum-requests=10000 python-path=/usr/local/lib/python2.7/dist-packages
    WSGIScriptAlias / /srv/www/app/apache/apache.wsgi
    WSGIProcessGroup python-app

    CustomLog "|/usr/bin/rotatelogs /srv/www/app/apache/logs/access.log.%Y%m%d-%H%M%S 5M" combined
    ErrorLog "|/usr/bin/rotatelogs /srv/www/app/apache/logs/error.log.%Y%m%d-%H%M%S 5M"
    LogLevel warn

    <Directory /srv/www/app>
        Order deny,allow
        Allow from all
        Require all granted
    </Directory>

</VirtualHost>
```

Make sure you create the /srv/www/app/apache/logs folder

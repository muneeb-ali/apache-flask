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

These instructions are for Debian, for other Linux flavors modify the httpd parts accordingly

1. Make sure httpd and mod_wsgi are installed

> sudo apt-get install -y apache2
> sudo apt-get install -y libapache2-mod-wsgi
> sudo a2enmod rewrite
> sudo apt-get -y apache2-utils

2. In /etc/apache2/sites-available/000-default.conf file add:
```
    <VirtualHost _default_:80>
    DocumentRoot /path/to/application

    WSGIDaemonProcess sandbox user=<username> group=<groupname> threads=15 maximum-requests=10000 python-path=/path/to/application/path/to/python/site-packages
    WSGIScriptAlias / /path/to/application/apache/apache_flask.wsgi
    WSGIProcessGroup <name_of_application>

    CustomLog "|/usr/sbin/rotatelogs /path/to/application/apache/logs/access.log.%Y%m%d-%H%M%S 5M" combined
    ErrorLog "|/usr/sbin/rotatelogs /path/to/application/apache/logs/error.log.%Y%m%d-%H%M%S 5M"
    LogLevel warn

    <Directory /path/to/application>
        Order deny,allow
        Allow from all
    </Directory>

    </VirtualHost>
```

3. Make sure you create the /path/to/application/apache/logs folder

4. Make sure that in WSGIDaemonProcess, the python-path=/path/to/application/path/to/python/site-packages points to the virtualenv that you created with "pip install -r requirements.txt"
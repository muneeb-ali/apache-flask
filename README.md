apache-flask
============

Python Flask (web framework) deployed with Apache

### Quick Start (using Docker)

For a quick demo, you can use a pre-built docker image: 

> docker run -d -p 80:80 muneeb/apache-flask 

The test page should be viewable on http://localhost. For Mac OS X, use boot2docker to get the IP of your VM:
> boot2docker ip 

And then view the test page at http://IP_FROM_BOOT2DOCKER e.g., http://192.168.59.103/

### Deploying Your App (using Docker)

1. Fork this repo

2. Clone your own version
> git clone git@github.com:YOUR_GITHUB_USERNAME/apache-flask.git

3. Make any modifications and commit them to github

4. Edit the apache-flask/image/Dockerfile to use your fork i.e., 
> replace 'RUN git clone https://github.com/muneeb-ali/apache-flask.git /srv/www/app' <br>
> with 'RUN git clone https://github.com/YOUR_GITHUB_USERNAME/apache-flask.git /srv/www/app'

5. Build the new docker image
> cd apache-flask/image <br>
> docker build -t=YOUR_TAG .

6. Run your new image similar to instructions above, replacing muneeb/apache-flask with YOUR_TAG

### Quick Start (without Docker)

1. fork this repo

2. clone your own version
> git clone git@github.com:YOUR_GITHUB_USERNAME/apache-flask.git <br>
> cd apache-flask

3. install all the necessary packages (best done inside of a virtual environment)
> pip install -r requirements.txt

4. run the app
> python runserver.py

5. check out the test page and make your modifications
> http://localhost:5000

### Deploying with Apache (without Docker)

These instructions are for Debian, for other Linux flavors modify the Apache parts accordingly

Make sure apache2 and mod_wsgi are installed

```
sudo apt-get install -y apache2
sudo apt-get install -y libapache2-mod-wsgi
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


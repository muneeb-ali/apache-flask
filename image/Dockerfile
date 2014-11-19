############################################################
# Dockerfile to build apache-flask container images
# Based on Ubuntu
############################################################

# Set the base image to Ubuntu
FROM ubuntu

# File Author / Maintainer
MAINTAINER Muneeb Ali (@muneeb)

# Update the repository sources list
RUN apt-get update

################## BEGIN INSTALLATION ######################

# Add the package verification key
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10

# Update the repository sources list once more
RUN apt-get update

# Install Apache2
RUN apt-get install -y \
	apache2 \
	apache2-utils \
	libapache2-mod-wsgi \
	git \

# Create the default data directory
RUN mkdir -p /srv/www

##################### INSTALLATION END #####################

# Expose the default port
EXPOSE 80

# Set default container command
ENTRYPOINT /usr/sbin/apache2ctl -D FOREGROUND
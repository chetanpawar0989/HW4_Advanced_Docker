FROM    chetanpawar0989/centos:hw4

# Enable EPEL for Node.js
# RUN     rpm -Uvh http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm
# Install Node.js and npm
# RUN     yum install -y npm

# Bundle app source
COPY ./app/ /src
# Install app dependencies
RUN cd /src; npm install

EXPOSE  8080
CMD ["node", "/src/main.js", "8080"]
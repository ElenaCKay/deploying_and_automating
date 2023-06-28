#!/bin/bash

# update
sudo apt update -y

# upgrade
sudo apt upgrade -y

# install the correct version of Mongo DB

wget -qO - https://www.mongodb.org/static/pgp/server-3.2.asc | sudo apt-key add -
echo "deb http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
sudo apt-get install -y mongodb-org=3.2.20 mongodb-org-server=3.2.20 mongodb-org-shell=3.2.20 mongodb-org-mongos=3.2.20 mongodb-org-tools=3.2.20

# configure the bindIp to 0.0.0.0 (Hint: use sed command)
cd /
sudo sed -i 's/127.0.0.1/0.0.0.0/g' /etc/mongod.conf
cd ~
# start and enable Mongo DB

sudo systemctl start mongod
sudo systemctl enable mongod

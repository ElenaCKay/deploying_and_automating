#!/bin/bash

# update
sudo apt update -y
echo "Update complete"

# upgrade
sudo apt upgrade -y
echo "Upgrade complete"

# install nginx
sudo apt install nginx -y

echo "nginx installed"

# restart nginx
sudo systemctl restart nginx

# enable nginx - make sure that when the virtual machine restarts nginx will automatically start
sudo systemctl enable nginx

# setup node
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -

# install node

sudo apt install nodejs -y
sudo npm install pm2 -g
echo "installed node and pm2"

# create env variable. Ensure the ip is changed to represent the correct db vm
export DB_HOST=mongodb://20.162.216.139:27017/posts
printenv DB_HOST
echo "env variable created"

# copy app.js folder

cd ~
git clone https://github.com/ElenaCKay/tech241_aparta_app.git sparta_app
echo "Sparta app cloned"
cd sparta_app
cd app

# run test app in the background

npm install
echo "Npm install success"
pm2 start app.js

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

# Change file for reverse proxy
sudo sed -i 's#try_files $uri $uri/ =404;#proxy_pass http://localhost:3000;#g' /etc/nginx/sites-available/default


# Test it is working 
sudo mginx -t
cat  /etc/nginx/sites-available/default

# setup node
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -

# install node

sudo apt install nodejs -y
sudo npm install pm2 -g
echo "installed node and pm2"

# create env variable. Ensure the ip is changed to represent the correct db vm
export DB_HOST=mongodb://34.245.15.198:27017/posts
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

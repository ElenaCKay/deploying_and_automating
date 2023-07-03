# Deploying and Automating

## SCP app folder

This must been run in the git bash terminal not in the VM. Chech which directory you are in as this will effect the path of the app folder in the command.

Example:

`scp -r -i ~/.ssh/tech241-elena-az-key app/adminuser@20.58.17.11:~/`

`scp -r -i ~/.ssh/<ssh private key> <app folder path>/adminuser@<app VM IP address>:~/`

## App Script:

To deploy and automate your application, follow these steps within the app script:

1. Always perform an update and upgrade of the system first to ensure it is up to date.

2. Install Nginx as a web server if it is not already installed.

3. If Git is not already installed, you may need to install it.

4. Install a specific version of Node.js using the command: curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -. Note that this command installs an older version of Node.js that is no longer supported.

5. Install Node.js by running: sudo apt install nodejs -y.

6. Install the pm2 package globally using npm: sudo npm install pm2 -g. pm2 is a node package manager that runs Node.js applications in the background.

7. Navigate to the app directory and run npm install. Note that this command will only work if Nginx is running.

8. Start the application either by running npm start or node app.js. The application will run and listen on port 3000.

These steps can be seen in the app_script.md file. \scripts\app_script.md

## Creating an Inbound Security Rule for Port 3000:

To allow inbound traffic on port 3000, follow these steps in the Azure portal:

1. Go to the Azure portal and navigate to the networking tab of your resource.

2. Add an inbound security rule.

3. Set the port to 3000.

4. Choose the TCP protocol.

5. Save the rule to apply the changes.

## Terminating the Application:

To terminate the application:

Use Ctrl + C to exit from the application's listening mode. Note that this action is more abrupt than using Ctrl + Z.

If you have used Ctrl + Z, it will close the terminal but keep the application running in the background. To completely stop it, you can use the kill -1 command.

Note that only one service can run on a specific port at a time. So, two services cannot use the same port simultaneously.

## Database VM

Steps to setup database:

1. Linux VM - Ubuntu 18.04 LTS
2. update and upgrade
3. install mongo db - version 3.2.x
   1. download key to get the right version
   2. source list - specify mongo db version
   3. update again
   4. install mongo db
4. configure mongo db to accept connections from app VM

Command steps:

    sudo apt update -y
    sudo apt upgrade -y
    wget -qO - https://www.mongodb.org/static/pgp/server-3.2.asc | sudo apt-key add -
    sudo apt update -y
    sudo apt-get install -y mongodb-org=3.2.20 mongodb-org-server=3.2.20 mongodb-org-shell=3.2.20 mongodb-org-mongos=3.2.20 mongodb-org-tools=3.2.20
    sudo nano /etc/mongod.conf

1. In the mongod.conf file, change the bind IP from 127.0.0.1 to 0.0.0.0 to allow outside connections. Note that this change is for testing purposes only.

2. Check if MongoDB is running using sudo systemctl status mongod.

3. Start MongoDB using sudo systemctl start mongod.

4. Enable MongoDB to start on boot using sudo systemctl enable mongod.
   
These steps can be seen in the db_script.md file. /scripts/db_script.md

## Connect app and database

Next you need to connect the app and the database.

On the app virtual machine you need to set up an environment variable.

    export DB_HOST=mongodb://<DB-IP-ADDRESS>:27017/posts #Example
    export DB_HOST=mongodb://20.162.216.139:27017/posts #Actual one I used
    npm install

We havent made a rule which allows things to connect to the app through this certain port.

So we need to add a new import rule in networking for the database to allow this port 27017.

Adding & would work the first time but then the second time you wouldnt be able to kill it as it would have a different process ID number.

## Adding in a reverse proxy server

To add a reverse proxy server, which affects the Nginx configuration file located on the app VM, follow these steps:

1. Go to the /etc/nginx/sites-available directory.

2. Open the default file using the command: sudo nano default.

3. Add the following configuration within the location / {} block:

     proxy_pass http://localhost:3000/;

![reverse proxy img](reverse_proxy_img.png)

![reverse proxy commands](<reverse proxy commands.png>)


## What have I learned from blockers?

- Make sure that the port 27017 is Custom
- Ensure the bind ip has changed to 0.0.0.0
- Make sure all the ip addresses match the azure ip addresses
- Remember to cd into the correct directories
- Don't forget the -i in the sed command
- Start the DB first and then the app
- The reverse proxy server has localhost in it not the ip

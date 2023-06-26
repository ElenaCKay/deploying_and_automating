# Deploying and Automating

1. Always do the update and upgrade first.
2. we will need nginx so we have a web server
3. maybe need git if it isnt already installed
4. `curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -` - This command is the version of node that we want. It is an older version which is no longer supported.
5. install node.js `sudo apt install nodejs -y`
6. `sudo npm install pm2 -g` node package manager pm2 runs node in the background
7. within the app directory `npm install` only works if nginx is running
8.  `npm start` or `node app.js`

Running and listening on port 3000

To allow the port (create a rule)
On portal azure - networking tab:
1. add inbound security rule
2. change port to 3000
3. protocol is TCP
4. add it

ctrl c to exit from listening -> More harsh than ctrl z.

ctrl z will close the terminal but carry on running in the background
- if you do this then kill it with kill -1 brute force
  
Only one thing can be running on a port. So two services cant use the same port. 

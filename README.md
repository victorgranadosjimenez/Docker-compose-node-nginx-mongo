##Docker container using a nodejs app + mongodb + nginx server

We have 3 main directories with their Dockerfile:
mongodb/Dockerfile
to create an image of mongodb
nginx/Dockerfile
to create an image of nginx
node/Dockerfile
to create an image of nodejs

We also have 2 config files
init.sh
get node project to deploy with docker
docker-compose.yml
file to orchestate the deployment of the app


##Install Docker

#Update the apt package index:

sudo apt-get update

#Install packages to allow apt to use a repository over HTTPS:

$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

#Add Docker’s official GPG key:

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -


#Verify that you now have the key with the fingerprint 9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88, by searching for the last 8 characters of the fingerprint.
sudo apt-key fingerprint 0EBFCD88


#Use the following command to set up the stable repository. To add the nightly or test repository, add the word nightly or test (or both) after the word stable in the commands below. Learn about nightly and test channels.

$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

#Update the apt package index.
sudo apt-get update


#Install the latest version of Docker Engine - Community and containerd, or go to the next step to install a specific version:

sudo apt-get install docker-ce docker-ce-cli containerd.io


#Verify that Docker Engine - Community is installed correctly by running the hello-world image.
This command downloads a test image and runs it in a container. When the container runs, it prints an informational message and exits.

sudo docker run hello-world


## we install docker-compose, give permission and chech is working

sudo curl -L "https://github.com/docker/compose/releases/download/1.25.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

docker-compose --version


##we install git

sudo apt-get install git


#start docker

sudo service docker start


## we download the app
init.sh script file download the node.js project and move it to folder "node/" and edited app.js to conect to mongodb container


chmod +x init.sh && ./init.sh


## launch containers with docker-compose

#we set port 80 in docker-compose.yml

#build the images

sudo docker-compose build

#run the containers

sudo docker-compose up


#go to localhost to check

#to set a domain we should edit  "nginx/nginx.conf" and "/etc/hosts"

for example
server_name node-app.dev www.node-app.dev in nginx.conf file
127.0.0.5	node-app.dev www.node-app.dev in /etc/hosts
go to http://node-app.dev

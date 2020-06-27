# Deployment Guide for Containerized App
file version: deployment  
This deployment guide is for App embedded with Docker Compose file which will be built in the EC2 instance.

## SSH to EC2 Instance:
* Set read permission to key file:
```bash
chmod 400 <pem-dir>
```

* SSH:
```bash
ssh -i <pem-dir> <ec2-user@public-dns>
```
## Configuration:
* Update package manager:
```bash
sudo yum update
```

* Install Docker Community Edition package:
```bash
sudo amazon-linux-extras install docker
```

* Start the Docker service:
```bash
sudo service docker start
```

* Add the ec2-user to the docker group:
```bash
sudo usermod -a -G docker ec2-user
```

* Install docker-compose
```bash
sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
```

* Fix docker-compose permission
```bash
sudo chmod +x /usr/local/bin/docker-compose
```

* Install Git:
```bash
sudo yum install git
```

* Install NVM:
```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash 
```

* Install Node:
```bash
nvm install <node-version>
```

* Install forever npm package:
```bash
npm install -g forever
```

* Mapping port:
```bash
sudo iptables -t nat -I PREROUTING -p tcp --dport 80 -j REDIRECT --to-ports <project-port>
```

## Deployment
* Clone project:
```bash
git clone <git-repo-url>
```

* Change working directory to project root directory:
```bash
cd <project-name>
```

* Switfh to target branch:
```bash
git checkout <target-branch-name>
```

* Configure .env file:
```bash
touch .env
vim .env
```

* Login to npm (If private npm packages were used):
```bash
npm login
```

* Install npm dependencies:
```bash
npm install
```

* Start App using node package forever:
```bash
forever start bin/www  
```  

## Clean Up
* Exit SSH:
```bash
exit
```
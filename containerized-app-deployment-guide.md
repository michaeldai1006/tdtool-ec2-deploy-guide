# Deployment Guide for Containerized App
file version: 0.1  
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

* Install docker-compose:
```bash
sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
```

* Fix docker-compose permission:
```bash
sudo chmod +x /usr/local/bin/docker-compose
```

* Create symbolic link for docker-compose:
```bash
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

* Install Git:
```bash
sudo yum install git
```

* Mapping port (If needed):
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

* Switch to target branch:
```bash
git checkout <target-branch-name>
```

* Start application:
```bash
docker-compose up -d
```

* Stop application:
```bash
docker-compose stop
```

## Clean Up
* Stop application, remove containers entirely:
```bash
docker-compose down
```

* Exit SSH:
```bash
exit
```
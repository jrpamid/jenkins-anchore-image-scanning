# Anchore (Container Image Vulnerability Scanning)
<br>


Anchore Engine is an open source project that provides a centralized service for inspection, analysis and certification of container images. The Anchore engine is provided as a Docker container image that can be run standalone, or within an orchestration platform such as Kubernetes, Docker Swarm, Rancher, Amazon ECS, and other container orchestration platforms.


## Installation :- Manual step by step configuration of Anchore Engine  and Anchore-CLI

```
#create dirs
mkdir -p /work/images/anchore-scanner/{config,db}
```

```
#download  default anchore engine configfile
cd  /work/images/anchore-scanner/config
curl -O https://raw.githubusercontent.com/anchore/anchore-engine/master/scripts/docker-compose/config.yaml
```

```
# download the anchore docker-compose.yml. This defines the anchore-db and anchore-engine docker services
cd /work/images/anchore-scanner
curl -O https://raw.githubusercontent.com/anchore/anchore-engine/master/scripts/docker-compose/docker-compose.yaml
```

```
# pull the official anchore container images from dockerhub anchore/anchore-engine  and postGreSql 
sudo docker-compose pull
sudo docker-compose up -d
```

```
# to scan using local cli, install pip package 
# install anchore cli python package (centos)
sudo yum updateinfo
sudo yum install -y python-pip
sudo pip install anchorecli

# add anchore-cli to PATH variable
export PATH=/root/.local/lib/python2.7/site-packages:$PATH
sudo  anchore-cli --version

# setup anchore-cli to reach the anchore-engine server
export ANCHORE_CLI_URL=http://localhost:8228/v1
export ANCHORE_CLI_USER=admin
export ANCHORE_CLI_PASS=foobar
```

### anchore-cli  commands and usage
```
# anchore-cli usage and commands
sudo anchore-cli --url http://localhost:8228/v1 --u admin --p foobar system status
sudo anchore-cli --url http://localhost:8228/v1 --u admin --p foobar image add jrpamid/nginx_centos:1.16.1
sudo anchore-cli --url http://localhost:8228/v1 --u admin --p foobar image list
sudo anchore-cli --url http://localhost:8228/v1 --u admin --p foobar syste feeds list
sudo anchore-cli image get jrpamid/nginx_centos:1.16.1 (get summary of the image)
sudo anchore-cli image vuln jrpamid/nginx_centos:1.16.1 <all | os | non-os>
sudo anchore-cli image content jrpamid/nginx_centos:1.16.1 <os |  java | npm | files >

## Anchore  logs location for troubleshooting
image scan progress/failure logs =/var/log/anchore/anchore-worker.log
engine sync logs = /var/log/anchore/anchore-policy-engine.log

```

# Anchore & Jenkins pipeline for  image scanning on Docker
<br>
[docs] https://wiki.jenkins.io/display/JENKINS/Anchore+Container+Image+Scanner+Plugin

Download the github project. (https:/github.com/jrpamid/jenkins-anchore-image-scanning.git) 
This contains the docker-compose.yaml file for starting up Jenkins, anchore-engine, postgreSql database and anchore-cli.


### Jenkins side configuration
```
# Install the jenkins anchore plugin 
Jenkins > Manage Jenkins > Manage Plugins > search for "Anchore Container Image Scanner Plugin "

# Configure the Anchore plugin to  reach the anchore-engine
Engine Url
Engine username = admin 
Engine passwrod = foobar
```



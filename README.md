WIP
# Remote-Docker-Enviorment
Leverage more resources within you dev environment by using a remote Docker host.

## Intro

Find your poor laptop fans spinning into enternity? Have some extra resources laying around (spare desktop/laptop/server) that you want to be able to leverage while maintaining your current workflow? 

Consider using a remote Docker host!


## Setup 

For this guide, we will be using a 2019 Intel Macbook Pro for the development workstation and a i9 9900k intel workstation running Ubuntu 22.04.1 , we will refer to as the `Remote Docker Host`. 


Lets set up remote docker host first!

### Remote Docker Host 

#### SSH Server

Setup a SSH Server 

```
sudo apt-get update
sudo apt-get install openssh-server
sudo systemctl enable ssh
sudo systemctl start ssh
```

### Docker Desktop

Setup Docker Desktop
[Download](https://desktop.docker.com/linux/main/amd64/docker-desktop-4.11.0-amd64.deb?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-linux-amd64) the latest release 

```
rm -r $HOME/.docker/desktop
sudo rm /usr/local/bin/com.docker.cli
sudo apt purge docker-desktop

sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
sudo apt install gnome-terminal
sudo apt-get update
sudo apt-get install ./docker-desktop-<version>-<arch>.deb
```

Optional: Set it to run as a service
```
systemctl --user start docker-desktop
```

### Retrieve local IP 

```
sudo apt-get install net-tools
ifconfig | grep 192
```

### Development Enviorment

#### SSH Keypair

```
ssh-keygen
```

Now send the keypair to the Remote Host using the local IP address retieved in the step above and your username on the Remote Docker Host
```
ssh-copy-id userid@hostname
```

It should ask you to enter the password. Then confirm sucess. 

#### Specify the Remote Docker Host

Replace `userid` and `hostname` with your user name and the local IP of the Remote Docker Host. 

```
export DOCKER_HOST=ssh://userid@hostname
```

**Note** If you dont set this up in your `.profile`, or equivalent, you will need to export this variable in each new terminal session.

## Usage

Now use docker like normal and enjoy a fan-free development workstation :) 



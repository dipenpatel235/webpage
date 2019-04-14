# Hello Application Demo 

## server requirements 
 1. jenkins server (ubuntu 18)
 2. web server (ubuntu 18)

*  Note: below  all commands related ubunutu 18

## server dependancy
```
apt-get install git unzip
apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
apt-get update
apt-get install docker-ce docker-ce-cli containerd.io
wget https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip
unzip ngrok-stable-linux-amd64.zip
mv ngrok /usr/bin
```

## jenkins installation
*  follow steps to install jenkins server
`https://linuxize.com/post/how-to-install-jenkins-on-ubuntu-18-04/`

### jenkins required plugin
```
Github Pull Request Builder
SSH Plugin
```
### jenkins credential configuration

#### Steps:
```
Jenkins > credential > System > Global Credential > Add Credential > web server ssh credential e.g. username : root password:123
```
#### Steps:
```
Jenkins > credential > System > Global Credential > Add Credential > dockerhub credential
```

## Jenkins Create Project for generate docker image

```
Enter an item name > pipeline > Build Triggers > GitHub hook trigger for GITScm polling > Pipeline > Pipeline Script From SCM > SCM  >  Git > Repository URL (https://github.com/dipenpatel235/webpage.git) > Save
```

## Jenkins Create Project for pull new docker image

#### Steps:
```
Enter an item name > Source Code Management > git > Repository URL (https://github.com/dipenpatel235/webpage.git) > Build triggers > GitHub hook trigger for GITScm polling > build > SSH Site(choose remote ssh) > command (paste below docker script commands) >  execute each line > Save
```

```
sleep 60
docker pull dipend/webpage
docker stop webpage
docker rm webpage
docker run --name webpage -d -p 80:80 dipend/webpage
```

## start ngrok for public ip to receive github webhook (follow this only if you have local ip)

`ngrok authtoken 5q2VZdxrGZYALSG5PZ7eB_A8DWf7eQZGwtw9KDyGoy`

*  above command will give you piblic URL e.g. `https://7008922a.ngrok.io` so copy that and paste it in mine repo (`https://github.com/dipenpatel235/webpage`) setting webhook



## Cloning the Repository on any server

```
$git clone https://github.com/dipenpatel235/webpage
```

*  change anything in html file for test then execute below command

```
git add .
git commit -am "test"
git push
```

*  Then Look it Build Started and after 40-50 sec open upur web server page url and see your html page change
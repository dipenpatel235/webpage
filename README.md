# Hello Application Demo 

## server dependancy

apt-get install git
apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
apt-get update
apt-get install docker-ce docker-ce-cli containerd.io

## jenkins installation
`https://linuxize.com/post/how-to-install-jenkins-on-ubuntu-18-04/`

### jenkins required plugin
```
Github Pull Request Builder
SSH Plugin
```
### jenkins required configuration

Manage Jenkins > Configure System > SSH Remote Host >  hostname (web server ip) > port (22) > credential

GitHub Pull Request Builder > https://7008972d.ngrok.io/github-webhook > 




## Jenkins Create Project for generate docker image
Enter an item name > pipeline > Build Triggers > GitHub hook trigger for GITScm polling > Pipeline > Pipeline Script From SCM > SCM  >  Git > Repository URL (https://github.com/dipenpatel235/webpage.git) > Save


## Jenkins Create Project for pull new docker image
Enter an item name > Source Code Management > git > Repository URL (https://github.com/dipenpatel235/webpage.git) > Build triggers > GitHub hook trigger for GITScm polling > build > SSH Site(choose remote ssh) > command (paster below data) >  execute each line > Save
```
sleep 60
docker pull dipend/webpage
docker stop webpage
docker rm webpage
docker run --name webpage -d -p 80:80 dipend/webpage
```

## Cloning the Repository

```
$git clone https://github.com/dipenpatel235/webpage
```

## Building Docker Image

```
$cd webpage
$docker build -t dipend/webpage .
```

## Running the Container

```
$docker run -d -p 80:80 dipend/webpage
```

## Jenkinsfile

```
node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("dipend/webpage")
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * Just an example */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}
```

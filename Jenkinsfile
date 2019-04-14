node {
    def app


    /* Clone Github Repo */
    stage('Clone repository') {
        checkout scm
    }

    /* Build Docker Image */
    stage('Build image') {
        app = docker.build("dipend/webpage")
    }

    /* Test Docker Image */
    stage('Test image') {
        app.inside {
            sh 'echo "Test success"'
        }
    }

    /* Push Docker Image to registery*/
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

node ('docker-agent') {
    def app
    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */
        checkout scm
    }
    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
       
        /*sh 'groups'*/
        app = docker.build("jassi1203/docker-jenkins-app")
    }
    stage('Test image') {
        /* Ideally, we would run a test framework against our image. */
        app.inside {
            sh 'echo "Tests passed"'
        }
    }
    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag. */
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
    stage('Clearnup') {
        /* Remove the local docker image. */
          /*sh 'docker rmi $app'*/      
    }
    stage ('Run Application') {
    /* Run the application using Docker images from Docker hub. */
          sh 'docker run -v ${PWD}:/app -v /app/node_modules -p 4201:4200 --name angularapp --rm jassi1203/docker-jenkins-app:latest'      
    }
}

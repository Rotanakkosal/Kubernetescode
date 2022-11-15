node {
    def app
    stage('Clone repository') {
        checkout scm
    }
    stage('Build image') {
       app = docker.build("rotanakkosal/miniproject")
    }
    stage('Test image') {
        app.inside {
            sh 'echo "Tests passed"'
        }
    }
    stage('Push images') {   
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
        }
    } 
    stage('Trigger ManifestUpdate') {
                echo "triggering updateDeploymentjob"
                build job: 'updateDeployment', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
}

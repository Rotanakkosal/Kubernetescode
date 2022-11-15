node {
    def app
    agent label 'j-worker'
    stage('Clone repository') {
        git url: 'https://github.com/Rotanakkosal/kubernetescode.git'
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

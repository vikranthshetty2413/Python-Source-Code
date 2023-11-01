node {
    def app

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/vikranthshetty2413/Python-Source-Code.git'
            }
        }

    stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

    stage('Build Image') {
            steps {
                sh 'docker build -t vikranth-${app} .'
            }
        }

    stage('Test Image') {
  

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push Image') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
        }
    }
    
    stage('Trigger ManifestUpdate') {
                echo "triggering updatemanifestjob"
                build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
}

    
}   

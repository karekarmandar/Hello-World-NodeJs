pipeline {
    agent {
        // we are using docker based agent for the node configuration
        docker {
            image 'node:16-alpine'
            args '-u root'
        }
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the project and installing node packages...'
                sh 'npm install'
            }
        }
        stage('Deploying Code in Production'){
            steps {
                input(message: 'Hello World!', ok: 'Submit')
                    sh '''
                        hostname
                        '''
            }
        }
    }
}

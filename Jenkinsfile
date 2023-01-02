pipeline {

    agent {
        docker {
            image 'node'
            args '-u root'
        }
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                sh 'npm install --save-dev @cyclonedx/cyclonedx-npm && npm install && cyclonedx-npm --output-format JSON --output-file bom.json'
            }
        }
    }
}

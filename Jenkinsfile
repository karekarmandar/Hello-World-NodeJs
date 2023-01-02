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
                sh 'cd /home && npm install --global @cyclonedx/cyclonedx-npm && npm install --save-dev @cyclonedx/cyclonedx-npm && npx --package @cyclonedx/cyclonedx-npm --call exit && npm install && cyclonedx-npm --output-format JSON --output-file bom.json'
            }
        }
    }
}

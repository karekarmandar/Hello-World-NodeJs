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
                sh 'npm install --global @cyclonedx/cyclonedx-npm && npm install --save-dev @cyclonedx/cyclonedx-npm && npx --package @cyclonedx/cyclonedx-npm --call exit && npm install && cyclonedx-npm --output-format JSON --output-file bom.json'
            }
        }
        stage('dependencyTrackPublisher') {
            steps {
                withCredentials([string(credentialsId: 'ex4tcaMkEGeQ5s4OPocRHxwBkbckP9M9', variable: 'dt-key')]) {
                    dependencyTrackPublisher artifact: 'target/bom.json', projectName: 'nodejs', projectVersion: 'my-version', synchronous: true, dependencyTrackApiKey: API_KEY, projectProperties: [tags: ['tag1', 'tag2'], swidTagId: 'my swid tag', group: 'my group']
                }
            }
        }
    }
}

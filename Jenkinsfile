pipeline {

    agent {
        // we are using docker based agent for the node configuration
        docker {
            image 'node:16-alpine'
            args '-u root'
        }
    }

    stages {
        // In this stage we are installaing and configuring bom using cyclonedx and along with that 
        // we are also installing the the depdencies required for the project(nodejs). cyclonedx will be creating a 
        // report(bom.json) which we will consume in the dependency track 
        stage('Build') {
            steps {
                echo 'Building...'
                sh 'npm install --global @cyclonedx/cyclonedx-npm && npm install --save-dev @cyclonedx/cyclonedx-npm && npx --package @cyclonedx/cyclonedx-npm --call exit && npm install && cyclonedx-npm --output-format JSON --output-file bom.json'
            }
        }

        // in this stage we are exporting the report to the depdency track dashboard and we will be able to see the results 
        // on out jenkins board as well
        stage('dependencyTrackPublisher') {
            steps {
                withCredentials([string(credentialsId: 'dt-key', variable: 'dt-key')]) {
                    dependencyTrackPublisher artifact: 'bom.json', projectName: 'nodejs', projectVersion: 'my-version', synchronous: true, dependencyTrackApiKey: 'ex4tcaMkEGeQ5s4OPocRHxwBkbckP9M9', projectProperties: [tags: ['tag1', 'tag2'], swidTagId: 'my swid tag', group: 'my group']
                }
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

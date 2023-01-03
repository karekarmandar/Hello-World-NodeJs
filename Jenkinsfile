pipeline {

    agent {
        docker {
            image 'node:16-alpine'
            args '-u root'
        }
    }

    stages {
        scm {
            git {
                remote {
                    github('test-owner/test-project')
                    refspec('+refs/pull/*:refs/remotes/origin/pr/*')
                }
                branch('${sha1}')
            }
        }
        triggers {
            githubPullRequest {
                admin('admin')
                admins(['admin'])
                userWhitelist('us@us.com')
                // userWhitelist(['me@me.com', 'they@they.com'])
                // orgWhitelist('my_github_org')
                // orgWhitelist(['your_github_org', 'another_org'])
                // cron('H/5 * * * *')
                // triggerPhrase('OK to test')
                // onlyTriggerPhrase()
                useGitHubHooks()
                permitAll()
                autoCloseFailedPullRequests()
                allowMembersOfWhitelistedOrgsAsAdmin()
                extensions {
                commitStatus {
                    context('deploy to staging site')
                    triggeredStatus('starting deployment to staging site...')
                    startedStatus('deploying to staging site...')
                    // statusUrl('http://mystatussite.com/prs')
                    completedStatus('SUCCESS', 'All is well')
                    completedStatus('FAILURE', 'Something went wrong. Investigate!')
                    completedStatus('PENDING', 'still in progress...')
                    completedStatus('ERROR', 'Something went really wrong. Investigate!')
                }
            }
        }
    }
        stage('Build') {
            steps {
                echo 'Building...'
                sh 'npm install --global @cyclonedx/cyclonedx-npm && npm install --save-dev @cyclonedx/cyclonedx-npm && npx --package @cyclonedx/cyclonedx-npm --call exit && npm install && cyclonedx-npm --output-format JSON --output-file bom.json'
            }
        }
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

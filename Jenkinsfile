pipeline {

    agent {
        docker {
            image 'node:16-alpine'
            args '-u root'
        }
    }

job('upstreamJob') {
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
            // admin('user_1')
            // admins(['user_2', 'user_3'])
            // userWhitelist('you@you.com')
            // userWhitelist(['me@me.com', 'they@they.com'])
            // orgWhitelist('my_github_org')
            // orgWhitelist(['your_github_org', 'another_org'])
            // cron('H/5 * * * *')
            // triggerPhrase('special trigger phrase')
            // onlyTriggerPhrase()
            useGitHubHooks()
            permitAll()
            autoCloseFailedPullRequests()
            displayBuildErrorsOnDownstreamBuilds()
            whiteListTargetBranches(['master','test', 'test2'])
            blackListTargetBranches(['master','test', 'test2'])
            whiteListLabels(['foo', 'bar'])
            blackListLabels(['baz'])
            allowMembersOfWhitelistedOrgsAsAdmin()
            extensions {
                commentFilePath {
                    commentFilePath("relative/path/to/file")
                }
                commitStatus {
                    context('deploy to staging site')
                    triggeredStatus('starting deployment to staging site...')
                    startedStatus('deploying to staging site...')
                    addTestResults(true)
                    statusUrl('http://mystatussite.com/prs')
                    completedStatus('SUCCESS', 'All is well')
                    completedStatus('FAILURE', 'Something went wrong. Investigate!')
                    completedStatus('PENDING', 'still in progress...')
                    completedStatus('ERROR', 'Something went really wrong. Investigate!')
                }
                buildStatus {
                    completedStatus('SUCCESS', 'There were no errors, go have a cup of coffee...')
                    completedStatus('FAILURE', 'There were errors, for info, please see...')
                    completedStatus('ERROR', 'There was an error in the infrastructure, please contact...')
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

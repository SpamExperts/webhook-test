pipeline {
    agent {
        label 'mail-autobuild-1'
    }
    stages {
        stage('Preparing Enviroment') {
            steps {
                    sh 'echo "StrictHostKeyChecking no" >>~/.ssh/config'                 
                    sh "npm install -g allure-commandline"
                    sh "npm install -g codeceptjs"
                    sh "sudo npm install -g puppeteer --unsafe-perm=true"
            }
        }
        stage('General Clean up') {
            steps {
                dir("/root/trunk/") {
                    sh 'env PYTHONPATH=. python testutils/cleanup.py'
                }
            }
        }
        stage('Run archiving tests') {
            steps {
                catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
                    dir("/root/workspace/spampanel/public/js/ng2/") {
                        sh "codeceptjs run --grep '@full-stack-archive'"
                    }
                }
            }
        }
        stage('Run log tests') {
            steps {
                catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
                    dir("/root/workspace/spampanel/public/js/ng2/") {
                        sh "codeceptjs run --grep '@full-stack-log'"
                    }
                }
            }
        }
        stage('search reports tests') {
            steps {
                catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
                    dir("/root/workspace/spampanel/public/js/ng2/") {
                        sh "codeceptjs run --grep '@full-stack-search-reports'"
                    }
                }
            }
        }
    }
}

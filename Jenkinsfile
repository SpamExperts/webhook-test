pipeline {
    agent {
        dockerfile{
            label "linux-tests"
        }
    }
    stages {
        stage("Package and Deploy Production") {
            when {
                branch 'master'
            }
            environment {
                // TODO replace with allowed users list
                allowedUsers = "admin"
            }
            steps {
                    script {
                        try {
                        timeout(time: 2, unit: "MINUTES") {
                            input(
                                message: "Do you want to approve the deploy in production?",
                                ok: "Yes",
                                submitterParameter: "approver",
                                submitter: allowedUsers
                            )
                        }
                        }catch (FlowInterruptedException) {
                            catchError(buildResult: 'SUCCESS', stageResult: 'ABORTED') {
                                exit(1)
                            }
                        return
                        }
                    echo "Am intrat pe aici"
                    }
                }
            }
        }
    post {
        always {
        deleteDir()
            }
        }
}

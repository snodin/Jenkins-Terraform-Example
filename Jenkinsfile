pipeline {
    agent any
    options {
        skipDefaultCheckout(true)
    }
    stages {
        stage('clean workspace') {
            steps {
                cleanWs()
            }
        }
        stage('checkout') {
            steps {
                checkout scm
            }
        }
        stage('terraform') {
            steps {
                sh '/opt/homebrew/bin/terraform apply -auto-approve -no-color'
            }
        }
        stage('Conditional Approval') {
            steps {
                script {
                    // Check if the current branch is 'main'
                    def branchName = sh(returnStdout: true, script: 'git rev-parse --abbrev-ref HEAD').trim()
                    if (branchName == 'main') {
                        input(message: 'Approval required', ok: 'Proceed', submitterParameter: 'APPROVER')
                    } else {
                        echo "Skipping approval for branch ${branchName}"
                    }
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}

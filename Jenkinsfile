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
            when {
                expression {
                    return env.BRANCH_NAME == 'main'
                }
            }
            steps {
                input(message: 'Approval required', ok: 'Proceed', submitterParameter: 'APPROVER')
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}

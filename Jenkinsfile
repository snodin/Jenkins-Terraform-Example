pipeline {
    agent any
    options {
        skipDefaultCheckout(true)
    }
    stages {
        stage('Print Branch Name') {
            steps {
                script {
                    try {
                        // Ensure workspace is configured for Git
                        checkout scm
                        def branchName = env.GIT_BRANCH ?: env.BRANCH_NAME
                        branchName = branchName.replaceFirst('^.*/(.*$)', '$1')
                        echo "Current branch: ${branchName}"
                    } catch (Exception e) {
                        error("Failed to retrieve the branch name: ${e.getMessage()}")
                    }
                }
            }
        }
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
                    def branchName = env.GIT_BRANCH ?: env.BRANCH_NAME
                    branchName = branchName.replaceFirst('^.*/(.*$)', '$1')
                    return branchName == 'main'
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

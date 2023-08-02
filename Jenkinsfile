pipeline {
    agent any
    options {
        skipDefaultCheckout(true)
    }
    stages {
        stage('Print Branch Name') {
            steps {
                script {
                    def branchName
                    try {
                        branchName = env.GIT_BRANCH
                        if (branchName == null || branchName.isEmpty()) {
                            // Fallback to the Git command
                            branchName = sh(returnStdout: true, script: 'git rev-parse --abbrev-ref HEAD').trim()
                        } else {
                            branchName = branchName.replaceFirst('^.*/(.*$)', '$1')
                        }
                    } catch (Exception e) {
                        error("Failed to retrieve the branch name.")
                    }
                    echo "Current branch: ${branchName}"
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
                    def branchName
                    try {
                        branchName = env.GIT_BRANCH
                        if (branchName == null || branchName.isEmpty()) {
                            // Fallback to the Git command
                            branchName = sh(returnStdout: true, script: 'git rev-parse --abbrev-ref HEAD').trim()
                        } else {
                            branchName = branchName.replaceFirst('^.*/(.*$)', '$1')
                        }
                    } catch (Exception e) {
                        error("Failed to retrieve the branch name.")
                    }
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


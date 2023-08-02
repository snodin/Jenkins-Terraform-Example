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
        stage('Approval for Terraform') {
            steps {
                input(message: 'Approval required before Terraform', ok: 'Proceed', submitterParameter: 'APPROVER')
            }
        }
    stage('tfsec') {
      steps {
        sh ' /usr/local/bin/docker run --rm -v "$(pwd):/src" aquasec/tfsec .'
      }
    }

        stage('terraform') {
            steps {
                sh '/opt/homebrew/bin/terraform apply -auto-approve -no-color'
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}

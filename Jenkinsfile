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
   
stage('tfsec') {
      steps {
        sh ' docker run --rm -v "$(pwd):/src" aquasec/tfsec .'
      }
    }
        
    stage('Approval for Terraform') {
            steps {
                input(message: 'Approval required before Terraform', ok: 'Proceed', submitterParameter: 'APPROVER')
            }
        }

        stage('terraform') {
            steps {
                bat 'terraform apply -auto-approve -no-color' 
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}

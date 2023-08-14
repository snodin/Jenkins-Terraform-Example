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


    stages {
        stage('tfsec') {
            steps {
                script {
                    // Utilizamos la ruta completa a Docker en Windows
                    def dockerCmd = 'C:\\Program Files\\Docker\\docker.exe'

                    // Ruta del directorio actual en Windows
                    def currentDir = pwd()

                    // Comando para ejecutar el contenedor Docker
                    def dockerRunCmd = "\"${dockerCmd}\" run --rm -v \"${currentDir}:/src\" aquasec/tfsec"

                    // Ejecutar el comando en un proceso de shell de Windows
                    bat(script: dockerRunCmd, returnStatus: true)
                }
            }
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

pipeline {
    agent any
    tools {
        jdk "OracleJDK17"
        maven "MAVEN3"
    }


    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: '', url: 'https://github.com/haleem00/complete-prodcution-e2e-pipeline.git'
            }
        }


         stage('Kubernetes deploy') {
                  agent {label 'KOPS'}
                    steps {
                      sh "helm upgrade --install --force productchart helm/productchart \
                          --set image=${DOCKERIMAGE}:latest \
                          --namespace prod"
                      }
                }

    }
}

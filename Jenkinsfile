pipeline {
    agent any
    tools {
        jdk "jdk-tool"
        maven "Maven3"
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

        stage("Build App") {
            steps {
                sh "mvn clean install -DskipTests"
            }
        }

        stage('Checkstyle Analysis') {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }

//         stage('Sonarqube Analysis') {
//             steps {
//               script {
//                 withSonarQubeEnv(credentialsId: 'sonartoken') {
//                   sh 'mvn sonar:sonar'
//
//                 }
//               }
//             }
//         }

        stage('Sonar Scanner') {
          steps {
            script {
              withSonarQubeEnv(credentialsId: 'sonartoken') {
                sh '''
                   mvn clean verify sonar:sonar \
                   -Dsonar.projectKey=product-key \
                   -Dsonar.projectName=product \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/main/java \
                   -Dsonar.tests=src/test/java \
                   -Dsonar.exclusions=src/test/java/**
                '''
                }
               }
            }
        }
    }
}

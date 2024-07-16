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

        stage('Sonar Scanner') {
            environment {
                sonarhome = tool 'sonartool'
            }
        steps {
            withSonarQubeEnv('sonar') {
                sh '''
                    ${sonarhome}/bin/sonar-scanner \
                    -Dsonar.projectKey=product-key \
                    -Dsonar.projectName=product \
                    -Dsonar.projectVersion=1.0 \
                    -Dsonar.sources=src/ \
                    -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                    -Dsonar.junit.reportsPath=target/surefire-reports/ \
                    -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                    -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml
                '''
                }
            }
        }
    }
}

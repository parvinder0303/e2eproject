pipeline {
  agent any
  tools {
  jdk "jdk-tool"
  maven "Maven3"
  }

  stages {
    stage("Cleanup Workspace"){
      steps {
        cleanWs()
      }
      }

      stage("Checkout from SCM"){
              steps {
                  git branch: 'main', credentialsId: '', url: 'https://github.com/haleem00/complete-prodcution-e2e-pipeline.git'
              }

          }

      stage("Build App"){
            steps {
              sh "mvn clean install -DskipTests"
            }
            }
  }
}
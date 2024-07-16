pipeline {
  agent ant
  tools {
  jdk "jdk-tool"
  maven "Maven3"
  }

  stages {
    stage(Cleanup Workspace){
      steps {
        cleanWs()
      }
      }

      stage(Build App){
            steps {
              sh "mvn clean package"
            }
            }
  }
}
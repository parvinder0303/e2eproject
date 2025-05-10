pipeline{
    agent any

    tools {
        jdk 'jdk 17'
        maven 'maven3'
    }

    environment {
       SCANNER_HOME=tool 'sonar-scanner'
    }

    stages{

        stage("Checkout from SCM"){
            steps {
                git branch: 'main', url: 'https://github.com/parvinder0303/e2eproject'
            }
        }

        stage("Build Application"){
            steps {
                sh "mvn clean package"
            }
        }

        stage("Test Application"){
            steps {
                sh "mvn test"
            }
        }

        stage("Sonarqube Analysis") {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh "mvn sonar:sonar"
                    }
                }
            }
        }

         stage('Build & Push Docker Image') {
            steps {
                script {
                 echo "Docker image Creation"
                 docker.withRegistry('', 'docker-cred') {
                 sh '''
                   		sudo docker build -t demoapp . # todo build the docker image
                   		sudo docker tag demoapp parvindersingh0303/demoapp:V1.0
                   		sudo docker push parvindersingh0303/demoapp:V1.0
                   	'''
                    }
                 echo "Docker image Creation and push to docker hud completed"
                }
            }
        }

        stage("Trivy Scan") {
            steps {
                script {

                }
            }

        }

        stage ('Cleanup Artifacts') {
            steps {
                script {
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
                }
            }
        }

        stage("Trigger CD Pipeline") {
            steps {
                script {
                    sh "curl -v -k --user admin:${JENKINS_API_TOKEN} -X POST -H 'cache-control: no-cache' -H 'content-type: application/x-www-form-urlencoded' --data 'IMAGE_TAG=${IMAGE_TAG}' 'https://jenkins.dev.dman.cloud/job/gitops-complete-pipeline/buildWithParameters?token=gitops-token'"
                }
            }
        }

    }


}

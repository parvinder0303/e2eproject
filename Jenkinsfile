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
                    def image = docker.build("${IMAGE_NAME}:${TAG}")
                    
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        image.push()
                        image.push('latest') // Optional: also push as latest
                    }
                }
            }
        }

        stage("Trivy Scan") {
            steps {
                script {
		   sh ('docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image parvinder0303/e2eproject:1.0.0-22 --no-progress --scanners vuln  --exit-code 0 --severity HIGH,CRITICAL --format table')
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

    post {
        failure {
            emailext body: '''${SCRIPT, template="groovy-html.template"}''', 
                    subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Failed", 
                    mimeType: 'text/html',to: "parvindersingh0303@gmail.com"
            }
         success {
               emailext body: '''${SCRIPT, template="groovy-html.template"}''', 
                    subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Successful", 
                    mimeType: 'text/html',to: "parvindersingh0303@gmail.com"
          }      
    }
    }

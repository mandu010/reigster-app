pipeline{
    agent {label 'Jenkins-Agent2'}
    tools{
        jdk 'Java17'
        maven 'Maven3'
    }
    environment{
            APP_NAME = "mandu_app_pipeline2"
            RELEASE = "1.0.0"
            DOCKER_USER = "mandu010"
            DOCKER_PASS = "docker_creds"
            IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }
    stages{
        stage("Cleanup Workspace"){
            steps{
                cleanWs()
            }
        }
        stage("Checkout from SCM"){
            steps{
                git branch: 'master', credentialsId: "github", url: "https://github.com/mandu010/reigster-app"
            }
        }
        stage("Build Application"){
            steps{
                sh "mvn clean package"
            }
        }
        stage("Test Application"){
            steps{
                sh "mvn test"
            }
        }
        stage("SonarQube Analysis"){
           steps{
	           script {
                withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token'){
                    sh "mvn sonar:sonar"
                }
               }	
           }
       }       
        stage("Build and Push Into Docker Hub"){
           steps{
	           script {
		sh "echo ${IMAGE_NAME}"
		sh "echo ${IMAGE_TAG}"
                docker.withRegistry('',DOCKER_PASS){
                    docker_image = docker.build "{$IMAGE_NAME}"
                }
                docker.withRegistry('',DOCKER_PASS){
                    docker_image.push("${IMAGE_TAG}")
                    docker_image.push('latest')
                }
               }	
           }
       }       
}
}

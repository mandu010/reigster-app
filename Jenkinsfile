pipeline{
    agent {label 'Jenkins-Agent2'}
    tools{
        jdk 'Java17'
        maven 'Maven3'
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
<<<<<<< HEAD
       }       
        stage("Quality Gate"){
           steps{
	           script {
=======
       }
       stage("Quality Gate"){
           steps {
                   script {
>>>>>>> 4899f9f239c51eacd43a7d52d6e80c3857ea410f
                waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
                }
               }	
           }
       }       
    }
}

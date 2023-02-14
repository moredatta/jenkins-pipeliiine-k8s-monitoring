pipeline{
    agent any
    tools {
    maven 'M3'
  }
    environment {
		DOCKERHUB_CREDENTIALS = credentials('docker')
	}
    stages {
         stage('Login') {
		    steps {
               cmd '/c echo $DOCKERHUB_CREDENTIALS_USR'
               cmd '/c echo $DOCKERHUB_CREDENTIALS_PSW'
		        bat 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR -p $DOCKERHUB_CREDENTIALS_PSW'
			}
		}
       stage('build && SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonarqube-9.8') {
                    // Optionally use a Maven environment you've configured already
                   
                        sh 'mvn clean package sonar:sonar'
                   
                }
            }
       }
       
        stage('Build Image') {
            steps {
                sh "docker build -t moredatta574/jenkins-demo ."
            }
        }
        stage('push Image') {
            steps {
                sh "docker push  moredatta574/jenkins-demo"
            }
        }
	    
        
        stage('Test') {
            steps {
                echo 'Successfully Pushed'
            }
        }
	 stage('pull Image') {
            steps {
                sh "docker pull prom/prometheus"
            }
        }   
	  
	    
       
    }
}

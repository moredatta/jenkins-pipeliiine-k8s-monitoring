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
               bat "cmd /c echo $DOCKERHUB_CREDENTIALS_USR"
               bat "cmd /c echo $DOCKERHUB_CREDENTIALS_PSW"
		        bat "cmd /cecho $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR -p $DOCKERHUB_CREDENTIALS_PSW"
			}
		}
	
      
       
        stage('Build Image') {
            steps {
               bat "cmd /c docker build -t moredatta574/jenkins-demo ."
            }
        }
        stage('push Image') {
            steps {
           bat "cmd /c docker push  moredatta574/jenkins-demo"
            }
        }
	    
        
        stage('Test') {
            steps {
                echo 'Successfully Pushed'
            }
        }
	 stage('pull Image') {
            steps {
              bat "cmd /c docker pull prom/prometheus"
            }
        }   
	  
	    
	
    }
}

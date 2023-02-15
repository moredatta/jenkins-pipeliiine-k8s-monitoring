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
               sh "echo $DOCKERHUB_CREDENTIALS_USR"
              sh "echo $DOCKERHUB_CREDENTIALS_PSW"
		       sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR -p $DOCKERHUB_CREDENTIALS_PSW"
			}
		}
	stage('Build'){
            steps{
               bsh  "mvn clean package"
            }
         }
        stage('SonarQube analysis') {
//    def scannerHome = tool 'SonarScanner 4.0';
        steps{
        withSonarQubeEnv('SonarQube-8.9.9') { 
        // If you have configured more than one global server connection, you can specify its name
//      sh "${scannerHome}/bin/sonar-scanner"
       sh "mvn sonar:sonar"
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
	  
	 stage("Kubernetes Create Deployment"){
      steps{
        script {
           withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'kubernetes', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
  	    sh  "kubectl apply -f demo.yml"
}
        }
      }
    }    
	
    }
}

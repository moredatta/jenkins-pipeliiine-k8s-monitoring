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
                sh 'echo $DOCKERHUB_CREDENTIALS_USR'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW'
		        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR -p $DOCKERHUB_CREDENTIALS_PSW'
			}
		}
       stage('build && SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonarQube-8.9.20') {
                    // Optionally use a Maven environment you've configured already
                   
                        sh 'mvn clean package sonar:sonar'
                   
                }
            }
       }
       
        stage('Build Image') {
            steps {
                sh "docker build -t moredatta574/java ."
            }
        }
        stage('push Image') {
            steps {
                sh "docker push  moredatta574/java"
            }
        }
	    
        
        stage('Test') {
            steps {
                echo 'Successfully Pushed'
            }
        }
	    
	stage('Deploy to kubernetes cluster') {
                                     steps {
                                               echo 'continuous deployment'
                                       withKubeConfig([credentialsId: 'config(2)(k8s)])
	    
       
    }
}

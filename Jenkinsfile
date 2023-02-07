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
				bat "cmd echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR -p  $DOCKERHUB_CREDENTIALS_PSW" 
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
        stage("Quality gate") {
            steps {
                waitForQualityGate abortPipeline: true
            }
        }
        stage('Build Image') {
            steps {
                bat "cmd docker build -t moredatta574/java ."
            }
        }
        stage('Run Image') {
            steps {
                bat "cmd docker run -d   --name flask_container moredatta574/java"
            }
        }
        
        stage('Test') {
            steps {
                echo 'Successfully Pushed'
            }
        }
       
    }
}

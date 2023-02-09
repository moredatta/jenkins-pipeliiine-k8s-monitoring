pipeline{
    agent any
    tools {
    maven 'M3'
  }
    environment {
	    	PROJECT_ID = 'plasma-ivy-373905'
        	CLUSTER_NAME = 'cluster-1'
        	LOCATION = 'us-central1-c'
        	CREDENTIALS_ID = 'My-Project-16902'
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
                sh "docker build -t moredatta574/jenkins ."
            }
        }
        stage('push Image') {
            steps {
                sh "docker push  moredatta574/jenkins"
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
	  stage('Deploy to GKE') {
            steps{
                 sh "sed -i 's/hello:latest/hello:${env.BUILD_ID}/g' test.yml"
                step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'test.yml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
            }
        }
	 
	    
       
    }
}

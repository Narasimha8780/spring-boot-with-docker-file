pipeline {
    agent any 
      tools {
    	maven "Maven3"
     
    } 
       
      stages {
        
        stage('Clone Repo') {
          steps {
	    checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Narasimha8780/spring-boot-docker-app1221211.git']])  
            sh 'rm -rf spring-boot-docker-app1221211'
            sh 'git clone https://github.com/Narasimha8780/spring-boot-docker-app1221211.git '
            }
        }
        
        stage('Build'){
		steps{
			sh "mvn clean package"
		}
	  }
       
        stage('Build Docker Image') {
            steps {
              sh 'docker build -t narasimha8780/springboot:latest .'
              }
        }
        stage('Push Docker image') {
            environment {
                DOCKER_HUB_LOGIN = credentials('docker')
            }
            steps {
                sh 'docker login --username=$DOCKER_HUB_LOGIN_USR --password=$DOCKER_HUB_LOGIN_PSW'
                sh    'docker push narasimha8780/springboot:latest'
            }
        }
          
        stage('K8S Deploy') {
            steps {
                withKubeConfig([credentialsId: 'k8s', serverUrl: '']) {
                    sh ('kubectl apply -f  kube.yaml')
                }
            }
        }
        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://3.108.67.219:30007'
          }
        }
        
      }
}

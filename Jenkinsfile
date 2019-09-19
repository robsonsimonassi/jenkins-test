pipeline {
    agent any
    
    environment {
	    registry = "nexus:8083/ubuntu:latest"
	    registryCredential = 'nexus-docker-user'
	    dockerImage = ''
	  }
 
    options {
        skipDefaultCheckout(true)
    }
 
    stages {
        stage('Checkout SCM') {
            steps {
                echo '> Checking out the source control ...'
                checkout scm
            }
        }
        stage('Docker Build') {
            steps {
		        sh 'docker build -t nexus:8083/ubuntu:latest .'
		      }
        }
        stage('Docker Push') {
            steps {
                sh 'docker push nexus:8083/ubuntu:latest'
		    }
        }
    }
}
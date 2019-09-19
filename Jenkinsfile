pipeline {
    agent any
    
    environment {
	    registry = "172.31.30.19:8083/ubuntu:latest"
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
		        sh 'docker build -t 172.31.30.19:8083/ubuntu:latest .'
		      }
        }
        stage('Docker Push') {
            steps {
                withDockerRegistry([ credentialsId: "nexus-docker-user", url: "http://172.31.30.19:8083" ]) {
		          sh 'docker push 172.31.30.19:8083/ubuntu:latest'
		        }
            }
        }
    }
}
pipeline {
    agent any
    
    environment {
	    registry = "nexus:8083/ubuntu"
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
                script {
		         dockerImage = docker.build registry + ":$BUILD_NUMBER"
		        }
            }
        }
        stage('Docker Push') {
            script {
	          docker.withRegistry( '', registryCredential ) {
	          	dockerImage.push()
	          }
	        }
        }
    }
}
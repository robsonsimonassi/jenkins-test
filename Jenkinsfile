pipeline {
    agent any
    
    environment {
	    registry = "nexus:8083/ubuntu"
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
		          docker.build registry + ":$BUILD_NUMBER"
		        }
            }
        }
        stage('Docker Push') {
            steps {
                script {
		          docker.push registry + ":$BUILD_NUMBER"
		        }
            }
        }
    }
}
pipeline {
    agent any
   
    stages {
        stage('Checkout SCM') {
            steps {
                echo '> Checking out the source control ...'
                checkout scm
            }
        }
        stage('Docker Build') {
            steps {
		        sh 'docker build -t ${REGISTRY}/ubuntu:latest .'
		      }
        }
        stage('Docker Push') {
            steps {
                withDockerRegistry([ credentialsId: "nexus-docker-user", url: "http://${REGISTRY}" ]) {
		          sh 'docker push ${REGISTRY}/ubuntu:latest'
		        }
            }
        }
    }
}

pipeline {
    agent any
 
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
                echo '> Docker build image ...'
                sh 'docker build -t nexus:8083/ubuntu .'
            }
        }
        stage('Docker Push') {
            steps {
                echo '> Docker Push ...'
                sh 'docker push nexus:8083/ubuntu'
            }
        }
    }
}
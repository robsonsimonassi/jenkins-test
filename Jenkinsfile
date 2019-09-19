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
                sh 'docker build -t ubuntu:1.0.0 .'
            }
        }
        stage('Docker Push') {
            steps {
                echo '> Docker Push ...'
                sh 'docker push ubuntu:1.0.0'
            }
        }
    }
}
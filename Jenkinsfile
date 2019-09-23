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
		        sh 'docker build -t ${REGISTRY_REPOSITORY}:${BUILD_NUMBER} .'
		      }
        }
        stage('Docker Push') {
            steps {
                withDockerRegistry([ credentialsId: "nexus-docker-user", url: "${REGISTRY_URL}" ]) {
		          sh 'docker push ${REGISTRY_REPOSITORY}:${BUILD_NUMBER}'
		        }
            }
        }
        
         stage('Sanity check') {
            steps {
                input "Does the staging environment look ok?"
            }
        }

        stage('Deploy - Production') {
           steps {
                sh 'export RANCHER_NEW_IMAGE=${REGISTRY_REPOSITORY}:${BUILD_NUMBER}'
				sh 'python rancher-upgrade.py'
            }
        }	
    
    }
}
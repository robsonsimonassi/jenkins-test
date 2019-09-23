pipeline {
    agent any
    
    environment {
        RANCHER_NEW_IMAGE = """
								${ sh(   returnStdout: true,
						    			 script: 'echo $REGISTRY_REPOSITORY:$BUILD_NUMBER'
						  			)
						  		}
						  	""".trim()
 
        RANCHER_ENVIRONMENT= 'envId'
		RANCHER_SERVICE_ID = 'serviceId'
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
		        sh 'docker build -t ${RANCHER_NEW_IMAGE} .'
		      }
        }
        stage('Docker Push') {
            steps {
                withDockerRegistry([ credentialsId: "nexus-docker-user", url: "${REGISTRY_URL}" ]) {
		          sh 'docker push ${RANCHER_NEW_IMAGE}'
		        }
            }
        }
     
        stage('Deploy - Production') {
           steps {
            	sh '
            	
            	
            		python /scripts/rancher-upgrade.py
            	
            	
            	'
            }
        }	
    
    }
}
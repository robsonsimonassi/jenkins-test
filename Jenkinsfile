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
		        sh 'docker build -t ${REGISTRY_TAG}:$BUILD_NUMBER .'
		      }
        }
        stage('Docker Push') {
            steps {
                withDockerRegistry([ credentialsId: "nexus-docker-user", url: "${REGISTRY_URL}" ]) {
		          sh 'docker push ${REGISTRY_TAG}:$BUILD_NUMBER'
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
                sh """
                	 curl -u "${RANCHER_ACCESS_KEY}:${RANCHER_SECRET_KEY}" \
						-X POST \
						-H 'Content-Type: application/json'  \
						-d '{
							   "inServiceStrategy":{
							      "batchSize":1,
							      "intervalMillis":2000,
							      "startFirst":false,
							      "type":"inServiceUpgradeStrategy",
							      "launchConfig":{
							         "instanceTriggeredStop":"stop",
							         "kind":"container",
							         "networkMode":"managed",
							         "privileged":false,
							         "publishAllPorts":false,
							         "readOnly":false,
							         "runInit":false,
							         "startOnCreate":true,
							         "stdinOpen":true,
							         "tty":true,
							         "vcpu":1,
							         "type":"launchConfig",
							         "imageUuid":"docker:${REGISTRY_TAG}:$BUILD_NUMBER"
							      }
							   },
							   "toServiceStrategy":null
							}' \
						"${RANCHER_URL}/v2-beta/projects/${RANCHE_PROJECT_ID}/services/${RANCHER_SERVICE_ID}/?action=upgrade" 
				   
				   """
            }
        }	
    
    }
}
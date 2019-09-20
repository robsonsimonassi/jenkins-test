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
                sh (
				    script: """
				      curl -0 -X POST "${SLACK_WEBHOOK_URL}" \
				      -H "Expect:" \
				      -H 'Content-Type: text/json; charset=utf-8' \
				      -d @- << EOF
				      {
				        "attachments": [
				          {
				            "color": "${status}",
				            "title": "${title}",
				            "title_link": "${title_link}",
				            "text": "${message}\\nBuild: ${env.BUILD_URL}"
				          }
				        ]
				      }
				      EOF
				      """
				  )
        }
        
        
    }
}



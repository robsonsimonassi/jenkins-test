pipeline { 
    agent any 

    stages { 
        stage('STAGE 00'){ 
            steps{
                echo "Pipeline Usando Jenkinsfile"
            }
        }

        stage('STAGE 01'){ 
            steps{
                echo "Pipeline Usando Jenkinsfile"
            }
        } 
    }
	
stage('Building image') {
    steps{
      script {
        docker.build registry + ":$BUILD_NUMBER"
      }
    }
  }		
 
} 

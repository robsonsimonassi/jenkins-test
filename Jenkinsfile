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
						-d '{"upgrade":{"type":"serviceUpgrade","inServiceStrategy":{"batchSize":1,"intervalMillis":2000,"startFirst":false,"type":"inServiceUpgradeStrategy","launchConfig":{"instanceTriggeredStop":"stop","kind":"container","networkMode":"managed","privileged":false,"publishAllPorts":false,"readOnly":false,"runInit":false,"startOnCreate":true,"stdinOpen":true,"tty":true,"vcpu":1,"type":"launchConfig","blkioWeight":null,"capAdd":[],"capDrop":[],"cgroupParent":null,"count":null,"cpuCount":null,"cpuPercent":null,"cpuPeriod":null,"cpuQuota":null,"cpuRealtimePeriod":null,"cpuRealtimeRuntime":null,"cpuSet":null,"cpuSetMems":null,"cpuShares":null,"dataVolumes":[],"dataVolumesFrom":[],"description":null,"devices":[],"diskQuota":null,"dns":[],"dnsSearch":[],"domainName":null,"healthInterval":null,"healthRetries":null,"healthTimeout":null,"hostname":null,"imageUuid":"docker:172.31.30.19:8083/test:49","ioMaximumBandwidth":null,"ioMaximumIOps":null,"ip":null,"ip6":null,"ipcMode":null,"isolation":null,"kernelMemory":null,"labels":{"io.rancher.container.pull_image":"always","io.rancher.scheduler.affinity:host_label":"host=bc-prod-rancher-tools-01"},"logConfig":{"type":"logConfig","config":{},"driver":null},"memory":null,"memoryMb":null,"memoryReservation":null,"memorySwap":null,"memorySwappiness":null,"milliCpuReservation":null,"oomScoreAdj":null,"pidMode":null,"pidsLimit":null,"ports":[],"requestedIpAddress":null,"secrets":[],"shmSize":null,"stopSignal":null,"stopTimeout":null,"system":false,"user":null,"userdata":null,"usernsMode":null,"uts":null,"version":"f64ce8b6-f364-440b-bfd9-cf31d9064190","volumeDriver":null,"workingDir":null,"dataVolumesFromLaunchConfigs":[],"networkLaunchConfig":null,"createIndex":null,"created":null,"deploymentUnitUuid":null,"externalId":null,"firstRunning":null,"healthState":null,"removed":null,"startCount":null,"uuid":null},"previousLaunchConfig":{"instanceTriggeredStop":"stop","kind":"container","networkMode":"managed","privileged":false,"publishAllPorts":false,"readOnly":false,"runInit":false,"startOnCreate":true,"stdinOpen":true,"tty":true,"vcpu":1,"type":"launchConfig","blkioWeight":null,"capAdd":[],"capDrop":[],"cgroupParent":null,"count":null,"cpuCount":null,"cpuPercent":null,"cpuPeriod":null,"cpuQuota":null,"cpuRealtimePeriod":null,"cpuRealtimeRuntime":null,"cpuSet":null,"cpuSetMems":null,"cpuShares":null,"dataVolumes":[],"dataVolumesFrom":[],"description":null,"devices":[],"diskQuota":null,"dns":[],"dnsSearch":[],"domainName":null,"healthInterval":null,"healthRetries":null,"healthTimeout":null,"hostname":null,"imageUuid":"docker:172.31.30.19:8083/test:44","ioMaximumBandwidth":null,"ioMaximumIOps":null,"ip":null,"ip6":null,"ipcMode":null,"isolation":null,"kernelMemory":null,"labels":{"io.rancher.container.pull_image":"always","io.rancher.scheduler.affinity:host_label":"host=bc-prod-rancher-tools-01"},"logConfig":{"type":"logConfig","config":{},"driver":null},"memory":null,"memoryMb":null,"memoryReservation":null,"memorySwap":null,"memorySwappiness":null,"milliCpuReservation":null,"oomScoreAdj":null,"pidMode":null,"pidsLimit":null,"ports":[],"requestedIpAddress":null,"secrets":[],"shmSize":null,"stopSignal":null,"stopTimeout":null,"system":false,"user":null,"userdata":null,"usernsMode":null,"uts":null,"version":"cb77db98-0fe4-4c3e-b2d0-d1fefa7c4959","volumeDriver":null,"workingDir":null,"dataVolumesFromLaunchConfigs":[],"networkLaunchConfig":null,"createIndex":null,"created":null,"deploymentUnitUuid":null,"externalId":null,"firstRunning":null,"healthState":null,"removed":null,"startCount":null,"uuid":null},"previousSecondaryLaunchConfigs":[],"secondaryLaunchConfigs":[]},"toServiceStrategy":null}}' \
						"${RANCHER_URL}/v2-beta/projects/${RANCHE_PROJECT_ID}/services/${RANCHER_SERVICE_ID}/?action=upgrade" 
				   
				   """
            }
        }	
    
    }
}
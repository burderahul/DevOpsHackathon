

def gitUrl = "https://github.com/burderahul/DevOpsHackathon.git"
	def mvnHome
	def registryHost = "35.193.189.172"
	def registryPort = "8081"
	def registryDockerPort = "8083"
	def registryUser = "admin"
	def registryPassword = "admin"
	def sonarHost = "104.198.182.112"
	def sonarPort = "9000"
	def kubernetesNamespace= "default"
	def gkeProjectId = "calcium-storm-252622"
	def gkeClusterName = "standard-cluster-2"
	def gkeProjectZone = "us-central1-a"

node{
	stage('Checkout') { 
		timestamps {
			cleanWs()
			withEnv(["gitUrl=$gitUrl"]) {
				// Checkout source code repository
				git "$gitUrl"
				mvnHome = tool 'Local-Maven'
			}
		}
	}
	
	stage('Build') {
		timestamps {
			// Run the maven build
			withEnv(["MVN_HOME=$mvnHome"]) {
				if (isUnix()) {
					jobStatus = sh(returnStatus: true, script: 'cd ${WORKSPACE}/src/sample-eureka_grp6/ && "$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean package')
					
					if(jobStatus != 0) {
						sh "exit 1"
					} 
				}
			}
		}
	}

}

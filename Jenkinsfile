

String repo ="https://github.com/burderahul/DevOpsHackathon.git"

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

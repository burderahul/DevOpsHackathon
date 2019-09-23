

def gitUrl = "https://github.com/burderahul/DevOpsHackathon.git"
	def mvnHome
	def registryHost = "34.66.179.28"
	def registryPort = "8081"
	def registryDockerPort = "8083"
	def registryUser = "admin"
	def registryPassword = "admin"
	def sonarHost = "104.198.182.112"
	def sonarPort = "9000"
	def kubernetesNamespace= "default"
	def gkeProjectId = "devops-project-253500"
	def gkeClusterName = "devops-cluster-1"
	def gkeProjectZone = "us-central1-a"
	def customImage =""

node{
	stage('Checkout') { 
		timestamps {
			cleanWs()
			withEnv(["gitUrl=$gitUrl"]) {
				// Checkout source code repository
				git "$gitUrl"
				mvnHome = tool 'localmaven'
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
/*	stage('Upload Build Artifact') {
    		timestamps{
    			// Upload war file to Nexus artifactory
    			try {
    				nexusPublisher nexusInstanceId: 'Nexus-Server', nexusRepositoryId: 'maven-releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: './src/sample-eureka_grp6/eureka-server/target/']], mavenCoordinate: [artifactId: 'eureka-server', groupId: 'net.javatutorial.tutorials', packaging: 'jar', version: '${JOB_NAME}-${BUILD_NUMBER}']]]

    			} catch(Exception ex) {
    				sh "exit 1"
    			}
    		}
    	}

*/

stage('Static Analysis') {
		timestamps {
			// Run maven command to perform static analysis & publish report in sonar server
			withEnv(["MVN_HOME=$mvnHome", "sonarHost=$sonarHost", "sonarPort=$sonarPort"]) {
				if (isUnix()) {
					jobStatus = sh(returnStatus: true, script: 'cd ${WORKSPACE}/src/sample-eureka_grp6/ && "$MVN_HOME/bin/mvn" sonar:sonar -Dsonar.projectKey=DevOpsHackathon -Dsonar.host.url="http://104.198.182.112:9000/sonar" -Dsonar.login=118996445fb54e4903164d212bae9d903d6de135')

					if(jobStatus != 0) {
						sh "exit 1"
					}
				}
			}
		}
	}
stage('Create and Push Image') {
		timestamps {
			try {

				/* withEnv(["MVN_HOME=$mvnHome", "registryHost=$registryHost", "registryPort=$registryPort", "registryDockerPort=$registryDockerPort", "registryUser=$registryUser", "registryPassword=$registryPassword"]) {
					if (isUnix()) {
						// Download war file from Nexus repository
						jobStatus = sh(returnStatus: true, script: '"$MVN_HOME/bin/mvn" dependency:get -DremoteRepositories=http://"${registryHost}":"${registryPort}"/repository/maven-releases/ -DgroupId=net.javatutorial.tutorials -DartifactId=SimpleServlet -Dversion=${JOB_NAME}-${BUILD_NUMBER} -Dpackaging=war -Ddest=${WORKSPACE}/docker/')

						if(jobStatus != 0) {
							sh "exit 1"
						}



						// Create & push Docker Image
						loginStatus = sh(returnStatus: true, script: 'docker login "${registryHost}":"${registryDockerPort}" -u "${registryUser}" -p "${registryPassword}"')
						if(loginStatus != 0) {
							sh "Login to Docker Registry failed"
							sh "exit 1"
						}
						def customImage =  docker.build("${registryHost}:${registryDockerPort}/simpleservlet:${JOB_NAME}-${BUILD_NUMBER}", "${WORKSPACE}/docker/")
						customImage.push()
					}
				} */

				 customImage =  docker.build("eureka6:${JOB_NAME}-${BUILD_NUMBER}", "${WORKSPACE}/src/sample-eureka_grp6/eureka-server/")
			} catch(Exception ex) {
				sh "exit 1"
			}
		}
	}

//	stage('Deploy') {
  //  		timestamps {
    //			try {
    //				withEnv(["kubernetesNamespace=$kubernetesNamespace", "gkeProjectId=$gkeProjectId", "gkeClusterName=$gkeClusterName", "gkeProjectZone=$gkeProjectZone" ]) {

    //					step([$class: 'KubernetesEngineBuilder',namespace: "$kubernetesNamespace" , projectId: "$gkeProjectId" , clusterName: "$gkeClusterName" , zone: "$gkeProjectZone" , manifestPattern: 'kube_deploy.yaml', credentialsId: "$gkeProjectId" , verifyDeployments: false])
    //				}
    //			} catch(Exception ex) {
    //				sh "exit 1"
    //			}
    //		}
    //	}

}
def tomcatImageName = 'appname-dev-image'
def containerNamePattern = 'appname-dev-server'
def containersNeeded = 2
// Code to deploy web application into docker containers - creating docker image with BUILD_NUMBER in between
dir('docker_files/') {
	// Stop and remove existing containers if any
	sh "docker stop \$(docker ps -a | awk '{print \$NF}' | grep ${containerNamePattern}) || exit 0"
	sh "docker rm \$(docker ps -a | awk '{print \$NF}' | grep ${containerNamePattern}) || exit 0"

	def startDate = new Date()
	formattedDTTM = startDate.format("yyyy-MM-dd HH:mm:ss")
	sh "echo '<h1> Last Deployed at ${formattedDTTM} </h1>' > LastDeployed.html"
	sh "docker build -t ${tomcatImageName}:${buildNumber} ."

	// bring up the containers using the image
	for (i=1; i <= containersNeeded; i++) {
		def portNumber = "999${i}"
		sh "docker run -d -p ${portNumber}:8080 --name ${containerNamePattern}_${i} ${tomcatImageName}:${buildNumber}"
	}
}*/

// Code to deploy web application into docker swarm
dir('docker_files/') {
	// Stop and remove existing stacks if any
	sh "docker stack rm appname-dev-tomcat || exit 0"
	sh "sleep 10s"

	// sh "docker rmi 127.0.0.1:5000/appname-dev-image:latest -f || exit 0"
	sh "docker images | grep appname-dev | awk '{print \$3}' | xargs docker rmi -f || exit 0"

	// Create LastDeployed.html file
	def startDate = new Date()
	formattedDTTM = startDate.format("yyyy-MM-dd HH:mm:ss")
	sh "echo '<h1> Last Deployed at ${formattedDTTM} </h1>' > LastDeployed.html"
	sh "docker build -t 127.0.0.1:5000/appname-dev-image:${buildNumber} ."

	// Deploy the stack
	sh "export BUILD_NUMBER=${buildNumber}" // this will be read in docker-compose.yml during deploy
	sh "docker stack deploy --compose-file docker-compose.yml appname-dev-tomcat || exit 0"
}

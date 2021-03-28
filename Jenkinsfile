node {
    // reference to maven
    // ** NOTE: This 'maven-3.5.2' Maven tool must be configured in the Jenkins Global Configuration.   
    def mvnHome = tool 'maven-3.6.3'

    // holds reference to docker image
    def dockerImage
    // ip address of the docker private repository(nexus)
 
    def dockerImageTag = "devopsexample${env.BUILD_NUMBER}"
    
    def dockerhub1 = 'dockerhub'
    
    stage('Clone Repo') { // for display purposes
      // Get some code from a GitHub repository
      //git 'https://github.com/felipemeriga/DevOps-Example.git'
	    git 'https://github.com/ssethia1/DevOps-Example.git'
      // Get the Maven tool.
      // ** NOTE: This 'maven-3.5.2' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'maven-3.6.3'
    }    
  
    stage('Build Project') {
      // build project via maven
      sh "'${mvnHome}/bin/mvn' clean install"
    }
		
    stage('Build Docker Image') {
      // build docker image
      dockerImage = docker.build("sumitsethia1/devopsexample:${env.BUILD_NUMBER}")
    }
   
    stage('Deploy Docker Image'){
      
      // deploy docker image to nexus
		
      echo "Docker Image Tag Name: ${dockerImageTag}"
	  
	 sh "docker stop devopsexample"
	  
	 sh "docker rm devopsexample"
	  
	 sh "docker run --name devopsexample -d -p 2222:2222 sumitsethia1/devopsexample:${env.BUILD_NUMBER}"
	  
	  // docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
	  docker.withRegistry('https://registry.hub.docker.com/sumitsethia1/devops_example', dockerhub1 ) {
          dockerImage.push("${env.BUILD_NUMBER}")
          dockerImage.push("latest")
        }
      
    }
}

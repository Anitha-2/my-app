node{
   stage('SCM Checkout'){
     git 'https://github.com/Anitha-2/my-app.git'
   }
   stage('maven-buildstage'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
    stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }
   stage('Build Docker Image'){
   sh 'docker build -t anitha1812/myweb:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u anitha1812 -p ${dockerPassword}"
    }
   sh 'docker push anitha1812/myweb:0.0.2'
   }
   stage('Nexus Image Push'){
   withCredentials([string(credentialsId: 'nexuspass', variable: 'nexuspassword')]) {
   sh "docker login -u admin -p ${nexuspassword} 18.212.83.79:2222"
   sh "docker tag anitha1812/myweb:0.0.2 18.212.83.79:2222/anil:1.0.0"
   sh 'docker push 18.212.83.79:2222/anil:1.0.0' 
   }
   }
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		// 
	}
    }
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest anitha1812/myweb:0.0.2' 
   }
}

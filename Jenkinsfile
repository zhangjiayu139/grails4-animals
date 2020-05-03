#!groovy

node {
	echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
	
   stage('checkout') {
      checkout scm
   }

   stage('check tools') {
      sh "pwd"
      sh "gradle --version"
      sh "chmod u+x ./grailsw"
      sh "./grailsw -version"
   }

   stage('clean') {
      sh "gradle clean"
   }

   stage('test') {
   	  try {
      	sh "gradle test"
   	  } catch (Exception e){
   	  	echo "test of ${env.BUILD_ID} failed on ${env.JENKINS_URL}"
   	  	throw e	
   	  } finally {
   	    junit 'build/test-results/**/*.xml'    
   	  }
   }

   stage('packaging') {
      sh "./grailsw test war"
   	  archiveArtifacts artifacts: 'build/libs/*.war', fingerprint: true
   }
}
node {

  try {
  
  stage('Code Checkout') { 
      // Get some code from a GitHub repository
      git 'https://github.com/uday7095/simplewebapp.git'
   }
   
	  
   stage('Unit Test') { 
       withEnv(['PATH+EXTRA=/opt/apache-maven-3.6.3/bin']) {
      // Get some code from a GitHub repository
        sh 'mvn clean compile'
        sh 'mvn test'
        }
   }
   
   stage('Test Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   }
   
   stage('Package & Deploy') {
       withEnv(['PATH+EXTRA=/opt/apache-maven-3.6.3/bin']) {
            sh 'mvn package'
            sh 'curl --upload-file target/simplewebapp.war "http://deployer:deployer@3.89.232.25:8080/manager/text/deploy?path=/byjenkins&update=true"'
       }
   }
   

} catch(e) {
	echo "Caught some exception"
	
  }  finally {
	echo "Finally Block"
	
}
    
}

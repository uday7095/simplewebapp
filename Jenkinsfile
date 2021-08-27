node {

  try {
  
  stage('Code Checkout') { 
      // Get some code from a GitHub repository
      git 'https://github.com/uday7095/simplewebapp.git'
   }
   
	 withMaven(
        // Maven installation declared in the Jenkins "Global Tool Configuration"
        maven: 'maven-3', // (1)
        // Use `$WORKSPACE/.repository` for local repository folder to avoid shared repositories
        mavenLocalRepo: '.repository', // (2)
        // Maven settings.xml file defined with the Jenkins Config File Provider Plugin
        // We recommend to define Maven settings.xml globally at the folder level using
        // navigating to the folder configuration in the section "Pipeline Maven Configuration / Override global Maven configuration"
        // or globally to the entire master navigating to  "Manage Jenkins / Global Tools Configuration"
        mavenSettingsConfig: 'my-maven-settings' // (3)
    ) {

      // Run the maven build
      sh "mvn clean verify"

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

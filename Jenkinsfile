
node{
   stage('SCM Checkout'){
     git ' https://github.com/daundeswapnil/devops-java-maven-code.git'
   }
   stage('Compile-Package'){
      // Get maven home path
      def mvnHome =  tool name: 'MAVEN', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
   }
   
   stage('SonarQube Analysis') {
        def mvnHome =  tool name: 'MAVEN', type: 'maven'
        withSonarQubeEnv(credentialsId: 'jenkins-sonar') { 
          sh "${mvnHome}/bin/mvn sonar:sonar"
        }
    }
    stage('Upload To Nexus') {
      def mvnHome =  tool name: 'MAVEN', type: 'maven'   
      sh "${mvnHome}/bin/mvn deploy"
      //nexusArtifactUploader credentialsId: 'nexus-admin', groupId: 'com.example', nexusUrl: 'localhost:8081/nexus', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '2.0-SNAPSHOT'
    }
}

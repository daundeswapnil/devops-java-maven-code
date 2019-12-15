
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
    }
}

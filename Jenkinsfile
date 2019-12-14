
node{
   stage('SCM Checkout'){
     git ' https://github.com/daundeswapnil/devops-java-maven-code.git'
   }
   stage('Compile-Package'){
      // Get maven home path
      def mvnHome =  tool name: 'MAVEN', type: 'maven'   
      sh "${mvnHome}/bin/mvn package"
   }
   
   stage('SonarQube Analysis') {
        def mvnHome =  tool name: 'MAVEN', type: 'maven'
        withSonarQubeEnv(credentialsId: 'sonar-token') { 
          sh "${mvnHome}/bin/mvn sonar:sonar"
          sh 'mv target/myweb*.war target/myweb.war'
        }
    }

    stage('Build Docker Image'){
     sh 'docker build -t localhost:8081/myweb:2.0.0 .'
     }

     stage('Upload To Nexus'){     
      withCredentials([string(credentialsId: 'nexus-admin', variable: 'nexusPwd')]) {
        sh "docker login -u admin -p ${nexusPwd} localhost:8081"
      }      
      sh "docker push localhost:8081/myweb:2.0.0"
     }
}
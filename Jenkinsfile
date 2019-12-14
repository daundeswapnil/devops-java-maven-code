
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
        }
    }

    stage('') {
      nexusArtifactUploader credentialsId: 'nexus-admin', groupId: 'com.example', nexusUrl: 'localhost:8081/nexus', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '2.0-SNAPSHOT'
    }
/*
    stage('Build Docker Image'){
     sh 'docker build -t localhost:8081/java-maven-junit-helloworld:2.0.0 .'
     }

     stage('Upload To Nexus'){     
      withCredentials([string(credentialsId: 'nexus-admin', variable: 'nexusPwd')]) {
        sh "docker login -u admin -p ${nexusPwd} localhost:8081"
      }      
      sh "docker push localhost:8081/java-maven-junit-helloworld:2.0.0"
     }*/
}
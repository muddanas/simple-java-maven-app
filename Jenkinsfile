pipeline {
 agent any
 
 
 stages {
       stage('Clone'){ 
 steps{ 
 git branch: 'master', url:"https://github.com/muddanas/simple-java-maven-app.git" 
 } 
 } 
  stage('sonar'){
   steps{
   sh 'mvn sonar:sonar -Dsonar.host.url=http://18.237.4.146:9000 -Dsonar.login=2598609e32a60bf2dd248db27d6f34e711c0e1f8'
   }
  }
 stage('Artifactory Configuration'){ 
 steps{ 
 rtServer( 
 id: "ARTIFACTORY_SERVICE", 
 url: "http://54.187.68.91:8081/artifactory", 
 credentialsId: "artifactory_srikanth" 
 ) 
 rtMavenDeployer( 
 id: "MAVEN_DEPLOYER", 
 serverId: "ARTIFACTORY_SERVICE", 
 releaseRepo: "libs-release-local", 
 snapshotRepo: "libs-snapshot-local" 
 ) 
 rtMavenResolver( 
 id: "MAVEN_RESOLVER", 
 serverId: "ARTIFACTORY_SERVICE", 
 releaseRepo: "libs-release", 
 snapshotRepo: "libs-snapshot" 
 ) 
 } 
 } 
 stage('Exec Maven'){ 
 steps{ 
 rtMavenRun( 
 tool: 'maven3.6.0', 
 pom: 'pom.xml', 
 goals: 'clean install', 
 deployerId: "MAVEN_DEPLOYER", 
 resolverId: "MAVEN_RESOLVER" 
 ) 
 } 
 } 
 stage('Publish build info'){ 
 steps{ 
 rtPublishBuildInfo( 
 serverId: "ARTIFACTORY_SERVICE" 
 ) 
 } 
 }  

   }
 }

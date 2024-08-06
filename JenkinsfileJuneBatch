node
{
   properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

   def mavenHome= tool name:"maven 3.9.8"
   //this is first stage 
   stage('git checkout')
   {
    git branch: 'development', credentialsId: 'c13d2b4c-1f45-454d-96b4-0f4dae4cc207', url: 'https://github.com/kkeducation12345/maven-web-app-project-kk-funda.git'
   }
   stage('Build')
   {
      sh "${mavenHome}/bin/mvn clean package"
   }
    stage('SonarQube report')
   {
      sh "${mavenHome}/bin/mvn clean sonar:sonar"
   }

   stage('Deploy artifactt')
   {
      sh "${mavenHome}/bin/mvn clean deploy"
   }

    stage('Deploy App to tomcat')
   {
  sshagent(['580bc4fc-5845-4383-9a2a-c7840039f481']) 
      {
   
      sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.233.200.135:/opt/apache-tomcat-9.0.91/webapps/"
    
      }
     
   }
   
}//node ending 


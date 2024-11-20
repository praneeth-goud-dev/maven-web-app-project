node 
{

properties([pipelineTriggers([pollSCM('* * * * *')])])
 
def mavenHome = tool name : "maven 3.9.8"

	stage('git checkout')
	{
	git branch: 'main', credentialsId: '54af9fb9-36cd-4da9-bca7-54ac929792f3', url: 								'https://github.com/praneeth-goud-dev/maven-web-app-project.git'
	}
	
	stage('Build package')
	{
	sh "${mavenHome}/bin/mvn clean package"
	}

	stage('sonar')
	{
	sh "${mavenHome}/bin/mvn clean sonar:sonar"
	}

	stage('nexus')
	{
	sh "${mavenHome}/bin/mvn clean deploy"
	}

	stage('deploy')
	{
	sshagent(['15872549-6327-4233-a5e0-2faae3a6c88a']) {
   	 sh "scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/scripted-pl/target/maven-web-application.war ec2-user@3.7.46.150:/opt/apache-tomcat-9.0.97/webapps/"
	}
	}
}

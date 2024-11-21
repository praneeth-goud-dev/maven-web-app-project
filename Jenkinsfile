node {

def mavenHome = tool name : "maven 3.9.8"

	try {
	
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
	
	
	} catch (e){

	// If there was an exception thrown, the build failed

	currentBuild.result = "FAILED"

	} finally {

	// Success or failure, always send notifications

	notifyBuild(currentBuild.result)

	}//node ending

def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESSFUL'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESSFUL') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}
}

node
{
    def mavenHome=tool name: "maven"
    stage('Checkout')
    {
        git branch: 'main', credentialsId: 'bd94d51c-6581-4c6e-b88c-d95b4452e918', url: 'https://github.com/praneeth-goud-dev/maven-web-app-project.git'
    }
    stage('Build')
    {
      sh "${mavenHome}/bin/mvn clean install"
    }
    stage('SonarQubeReport')
    {
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage('Deploy Artifact to Nexus')
    {
        sh "${mavenHome}/bin/mvn deploy"
    }
    stage('Deploy App to tomcat')
    {
            sshagent(['13b55079-5954-4296-aeaa-c3f247dcaa2a']) {
                   sudo sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war  ec2-user@172.31.2.57:/opt/apache-tomcat-9.0.107/webapps/"
            }
    }
}

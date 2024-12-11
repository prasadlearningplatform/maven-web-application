
def sendSlackNotification(String buildStatus = 'STARTED') {
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
Complete snipp


node{
    
def mavenHome = tool name: 'Maven3.9.9'
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])    
try{
    stage('ChekoutCode'){
git credentialsId: 'GajulaPrasad', url: 'https://github.com/prasadlearningplatform/maven-web-application.git'
}
stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}
//sonarQube
stage('ExecuteSonarQube'){
sh "${mavenHome}/bin/mvn sonar:sonar"
}
//Artifactsnexus
stage('UploadArtifatsintonexus'){
sh "${mavenHome}/bin/mvn deploy"
}
//Tomcat
stage('DeployapplicationintoTomcatserver'){
sshagent(['983f212c-4c30-4f84-b553-23c0f8ca7034']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@54.88.60.52:/opt/apache-tomcat-9.0.97/webapps/"
} 
}
}
catch(e){
CurrentBuild.result = "FAILED"
}
finally{
sendSlackNotification(CurrentBuild.result)
  }
}

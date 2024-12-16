pipeline{
agent any
tools {
  maven 'Maven3.9.9'
}
options {
  timestamps()
}

stages{
stage('CheckoutCode'){
steps{
    sendSlackNotification('STARTED')
git credentialsId: 'GajulaPrasad', url: 'https://github.com/prasadlearningplatform/maven-web-application.git'
}
}
stage('Build'){
steps{
sh "mvn clean package"
}
}
/*
stage('ExecuteSonarQubeReport'){
steps{
sh "mvn sonar:sonar"
}
}
stage('UploadArtifactsintoNexusServer'){
steps{
sh "mvn deploy"
}
}
stage('DeployAPPintoTomcatServer'){
steps{
sshagent(['983f212c-4c30-4f84-b553-23c0f8ca7034']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@34.207.158.231:/opt/apache-tomcat-9.0.97/webapps/"
}

}
}
}//stage cl
*/
post {
success {
sendSlackNotification(currentBuild.result) 
}
failure {
sendSlackNotification(currentBuild.result)
}
}
}//pipeline cl
//uuu

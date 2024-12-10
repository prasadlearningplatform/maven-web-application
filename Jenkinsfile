node{
    
def mavenHome = tool name: 'Maven3.9.9'
    
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

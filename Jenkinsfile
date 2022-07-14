node{
def mavenHome = tool name: "maven3.8.5"
echo "Then jenkins path: ${env.JENKINS_HOME}"
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])

stage('CheckoutCode'){
git branch: 'development', credentialsId: '6f1b45e5-b6b3-4188-bc4d-8d481170ef3a', url: 'https://github.com/SirisOrg/maven-web-application.git'
}
stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}
/*
stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('DeployAppIntoTomcatServer'){
sshagent(['f0d6ff07-903d-4e09-880c-72d021f8fcd7']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.235.115.37:/opt/apache-tomcat-9.0.64/webapps/"
}
}
*/
}

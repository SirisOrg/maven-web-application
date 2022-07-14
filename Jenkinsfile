node{
def mavenHome = tool name: "maven3.8.5"
echo "Then jenkins path: ${env.JENKINS_HOME}"
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])

try{
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
catch(e){
currentBuild.result = "FAILURE"
throw e
    } 
    finally{
    notifyBuild(currentBuild.result)
    }
}
def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

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

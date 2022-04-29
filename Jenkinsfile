def sendSlackNotification(String buildStatus = 'STARTED') {
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
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}

node{
    
    echo "Job Name is  : ${env.JOB_NAME}"
    echo "Node Name is : ${env.NODE_NAME}"
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

def mavenHome = tool name: 'maven3.8.5'
try
{
//here we have to write script to get code from github
//Get the code from GitHub Repo
stage('CheckoutCode'){

git branch: 'development', credentialsId: '28d4be2d-8e51-4183-9752-ea1002f5ea81', url: 'https://github.com/gsrilalitha92/maven-web-application.git'
}
//Do the build by using Maven Build Tool
stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

/*
//Execute sonarqube report
stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn sonar:sonar"
}

//Upload Artifact into Artifactory Repo
stage('Upload Artifacts into Nexus'){
sh "${mavenHome}/bin/mvn deploy"
}

//Deploying Application Into Tomcat Server
stage('Deploying into Tomcat'){
sshagent(['11754853-4ad5-43ac-a7f0-62a61ee4d039']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@35.154.14.12:/opt/apache-tomcat-9.0.62/webapps/"
   
}
}
*/
}//try closing
    
 catch (e) 
{
  currentBuild.result = 'FAILURE'      
    }
 finally {
 sendSlackNotification(currentBuild.result)
}
    
}//closing node

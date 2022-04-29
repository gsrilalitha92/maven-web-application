node{
    
    echo "Job Name is  : ${env.JOB_NAME}"
    echo "Node Name is : ${env.NODE_NAME}"

def mavenHome = tool name: 'maven3.8.5'

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
}//closing node

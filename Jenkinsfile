node{

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

echo "job name is:${env.JOB_NAME}"
echo "Build number is:${env.BUILD_NUMBER}"
echo "Jenkins Home is:${env.JENKINS_HOME}"
echo "Build number is:${env.BUILD_NUMBER}"

def mavenHome = tool name : 'maven3.9.7'

stage('CheckOutCode'){
git branch: 'developement', credentialsId: 'bcb7566c-6fec-4365-a54a-6fba17e24620', url: 'https://github.com/shouvickaich/kiyansh123.git'
}
stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}
stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn clean sonar:sonar"
}

stage('uploadArtifactIntoNexus'){
sh "${mavenHome}/bin/mvn clean deploy"
}


stage('DeployAppIntoTomcat'){
sshagent(['2c89b0e9-dd6c-4701-94f0-dd8ab3ddbacb']) {
 sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.37.19:/opt/apache-tomcat-9.0.89/webapps/"  
 
}
}


}

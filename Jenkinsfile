node
{
    def mavenHome = tool name: "maven3.8.1"
    
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
    stage('checkoutcode')
{
git branch: 'development', credentialsId: 'c523da4f-f731-4358-8b85-00346814ebc1', url: 'https://github.com/mss-ec-apps-may/maven-web-application.git'
}
stage('build')
{
sh "${mavenHome}/bin/mvn clean package"
}

stage('ExcuteSonarQubeReport')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}
stage('uploadArtifactintoNexus')
{
sh "${mavenHome}/bin/mvn deploy"
}
stage('DeployApplicationintoTomcatServer')
{
sshagent(['c6a0a511-a6a9-4ebb-a3a4-91da952845ed']){
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@18.223.247.231:/opt/apache-tomcat-9.0.45/webapps/"
}
}

stage('SendEmailNotification')
{
emailext body: '''Build over...


Reguards,
Mithun Technologies,
9581041335''', subject: 'Build over...', to: 'devopstrainingfeb@gmail.com'
}
}

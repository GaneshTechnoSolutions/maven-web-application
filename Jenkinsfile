node
{
     echo "GitHub BranhName ${env.BRANCH_NAME}"
  echo "Jenkins Job Number ${env.BUILD_NUMBER}"
  echo "Jenkins Node Name ${env.NODE_NAME}"
  
  echo "Jenkins Home ${env.JENKINS_HOME}"
  echo "Jenkins URL ${env.JENKINS_URL}"
  echo "JOB Name ${env.JOB_NAME}"
  
    properties([
    buildDiscarder(logRotator(numToKeepStr: '3')),
    pipelineTriggers([
        pollSCM('* * * * *')
    ])
  ])
   
    def mavenHome=tool name: "maven3.6.3"
    stage('Checkout')
    {
        git branch: 'development', credentialsId: 'c7580aef-8942-4fa0-9688-9dc8b0e5acb7', url: 'https://github.com/GaneshTechnoSolutions/maven-web-application.git'
    }
    
    stage('build')
    {
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage('Execute Sonarqube report')
    {
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage('UploadArtifactintoNexus')
    {
        sh  "${mavenHome}/bin/mvn deploy"
    }
    stage('DeployAppintoTomcat')
    {
     sshagent(['959c9dc1-9651-448d-8509-1d5dbe8f351f']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@35.163.196.46:/opt/apache-tomcat-9.0.29/webapps/"
}
    }
    stage('SendEmailNotification')
    {
        emailext body: '''
Regards,
S.Ganesh
GaneshTechnologies
Ph:No:9985255485''', subject: 'Build Deployment', to: 'Singadeganesh18@gmail.com'
    }
}

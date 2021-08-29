node
{
def mavenHome = tool name: "maven 3.8.2"

      echo "GitHub BranhName ${env.BRANCH_NAME}"
      echo "Jenkins Job Number ${env.BUILD_NUMBER}"
      echo "Jenkins Node Name ${env.NODE_NAME}"
  
      echo "Jenkins Home ${env.JENKINS_HOME}"
      echo "Jenkins URL ${env.JENKINS_URL}"
      echo "JOB Name ${env.JOB_NAME}"
stage('checkoutocode')
{
git branch: 'development', credentialsId: 'a3d4ada4-a254-40fe-89db-625a12a66eb1', url: 'https://github.com/devops-ec-projects/maven-web-application.git'
}
stage('Build') 	
{
sh "${mavenHome}/bin/mvn clean package"
}

    stage('SonarQubeReport'){
        sh "${mavenHome}/bin/mvn clean sonar:sonar"
    }

    stage('UploadArtifactintoNexus'){
        sh "${mavenHome}/bin/mvn clean deploy"
    }

    stage('DeployAppIntoTomcatServer')
{
sshagent(['01418ed7-a3c1-4c29-9dc4-627bbbdc447f']) {
  sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.233.57.77:/opt/apache-tomcat-9.0.52/webapps/"
}
}

stage('SendEmailNotification'){
emailext body: '''Build is over !!

Regards,
pavyasree''', subject: 'Build Over...!!', to: 'pavyavemaraj1997@gmail.com'
}
    
}

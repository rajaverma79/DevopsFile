node ('Slave')
{
    
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
     def mavenHome = tool name: "Maven 3.6.3"

  stage("CheckOutCode")
   {
    git branch: 'development', credentialsId: 'GIT_CREDENTIALS', url: 'https://github.com/rajaverma79/maven-web-application.git'
    }

  stage("Build")
   {
   sh "${mavenHome}/bin/mvn clean package"
   }
   
  
    /*
    stage("ExcuteSonarQubeReport")
   {
    sh "${mavenHome}/bin/mvn sonar:sonar"
   }
   */
   
   stage("UploadArtifactIntoNexus")
   {
   sh "${mavenHome}/bin/mvn deploy"
   }
   
   
    stage("DeployApplicationIntoTomcat") 
   {
     sshagent(['f84ccde4-9768-4e27-85e8-8daed07bbbb8']) 
     {
   sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.233.63.117:/opt/apache-tomcat-9.0.34/webapps"
    }
   }
      
}

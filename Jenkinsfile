node ('master')
 {
  
  def mavenHome = tool name: "mvn3.6.3"
  
      echo "GitHub BranhName ${env.BRANCH_NAME}"
      echo "Jenkins Job Number ${env.BUILD_NUMBER}"
      echo "Jenkins Node Name ${env.NODE_NAME}"
  
      echo "Jenkins Home ${env.JENKINS_HOME}"
      echo "Jenkins URL ${env.JENKINS_URL}"
      echo "JOB Name ${env.JOB_NAME}"
  
 
    properties([[$class: 'JiraProjectProperty'], buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    def mvnHome
    stage('Preparation') { // for display purposes
        git credentialsId: '5906d0e3-9816-404e-9a71-4769555aac72', url: 'https://github.com/param-tst/maven-web-application.git'
        // Get the Maven tool.
        // ** NOTE: This 'M3' Maven tool must be configured
        // **       in the global configuration.
        mvnHome = tool 'mvn3.6.3'
    }
    stage('Build') {
        // Run the maven build
        withEnv(["MVN_HOME=$mvnHome"]) {
            if (isUnix()) {
                sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean package'
            } else {
                bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
            }
        }
    }
     
     stage('sonar') {
        // Run the maven build
        withEnv(["MVN_HOME=$mvnHome"]) {
            if (isUnix()) {
                sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean sonar:sonar package'
            } else {
                bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
            }
        }
    }
  
     stage('tomcat-deploy') {
       sshagent(['5b5f1a66-d425-4c21-ac7b-d1cfad441db1']) {
             sh 'scp -o StrictHostKeyChecking=no target/maven-web-application.war centos@52.33.192.99:/opt/tomcat9/webapps'
    // some block
}
       
    }
    stage('nexus') {
        // Run the maven build
        withEnv(["MVN_HOME=$mvnHome"]) {
            if (isUnix()) {
                sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean deploy'
            } else {
                bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
            }
        }
    }
}

 
  stage("DeployAppTomcat")
 {
  sshagent(['423b5b58-c0a3-42aa-af6e-f0affe1bad0c']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war  ec2-user@15.206.91.239:/opt/apache-tomcat-9.0.34/webapps/" 
  }
 }
 
 stage('EmailNotification')
 {
 mail bcc: 'devopstrainingblr@gmail.com', body: '''Build is over

 Thanks,
 Mithun Technologies,
 9980923226.''', cc: 'devopstrainingblr@gmail.com', from: '', replyTo: '', subject: 'Build is over!!', to: 'devopstrainingblr@gmail.com'
 }
 */
 
 }

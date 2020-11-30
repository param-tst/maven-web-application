node('slaves') {
    properties([[$class: 'JiraProjectProperty'], buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    def mvnHome
    stage('Preparation') { // for display purposes
        git branch:'master',credentialsId: '5906d0e3-9816-404e-9a71-4769555aac72', url: 'https://github.com/param-tst/maven-web-application.git'
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
     
    
  
     stage('tomcat-deploy') {
       sshagent(['5b5f1a66-d425-4c21-ac7b-d1cfad441db1']) {
             sh 'scp -o StrictHostKeyChecking=no target/maven-web-application.war centos@34.216.166.134:/opt/tomcat9/webapps'
    // some block
}
       
    }
    
}

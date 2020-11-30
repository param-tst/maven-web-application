node('slaves') {
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
       configFileProvider([configFile(fileId: 'cfc2199f-8a69-4aa9-ac58-c8192266989f', targetLocation: '/home/ec2-user/slave1/tools/hudson.tasks.Maven_MavenInstallation/mvn3.6.3/conf/settings.xml', variable: 'MY_SETTINGS')]) {
    // some block

        withEnv(["MVN_HOME=$mvnHome"]) {
            if (isUnix()) {
                sh '"$MVN_HOME/bin/mvn" -s $MY_SETTINGS -Dmaven.test.failure.ignore clean package'
            } else {
                bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
            }
        }
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

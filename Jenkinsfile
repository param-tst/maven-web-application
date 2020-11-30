node {
    properties([[$class: 'JiraProjectProperty'], buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    def mvnHome;
    def SSH_OPTS="ssh  -o StrictHostKeyChecking=no";
    def USERNAME="centos";
    def IPADDRESS="34.216.166.134";
    
     stage('make-tar') {
         
       
         sh '''#!/bin/bash
               cd /var/lib
               pwd
               sudo tar -cf  jenkins.tar ./jenkins
               sudo chown -R jenkins:jenkins ./jenkins.tar
                ls -lart|grep jenkins.tar
         '''
   
       
        
      
       
    }
    stage('migration') {
         
         
       sshagent(['5b5f1a66-d425-4c21-ac7b-d1cfad441db1']) {
           
             sh '''#!/bin/bash
             scp -o StrictHostKeyChecking=no /var/lib/jenkins.tar centos@34.216.166.134:/home/centos/
             ssh  -o StrictHostKeyChecking=no  $USERNAME@$IPADDRESS "sudo cp -r /home/centos/jenkins.war /var/lib/"
              $SSH_OPTS $USERNAME@$IPADDRESS "sudo chown jenkins:jenkins -R /var/lib/jenkins.war"
            // $SSH_OPTS $USERNAME@$IPADDRESS "sudo tar -xvf /var/lib/jenkins.war"
             
              '''
}
       
    }
  
}

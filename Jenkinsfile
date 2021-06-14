pipeline {
    agent any
    tools {
        git 'Default'
        maven 'MAVEN_HOME' 
        jdk 'jdk1.8' 
        
    }
    stages {
    


        stage('SCM Checkout'){
            steps {
               git branch: 'qa', url: 'https://github.com/aksh153026/back-end.git'

            }
        }
        stage ('Code Quality scan')  {
        
         tools {
         jdk "jh" 
        }
    
        steps {
         script  {
            scannerHome = tool 'sonar2' // the name you have given the Sonar Scanner (Global Tool Configuration)
        }
    
        withSonarQubeEnv('sonarmvn') {
            
            bat "mvn clean intall package  &&  mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar"
        }
         }
       
    }
    
    }
	node("vagrant2"){
    
stage("com"){
    sh """#!/bin/bash
        rm -r scrum-ui
        git clone https://github.com/aksh153026/scrum-ui.git -b qa
        """
}

}
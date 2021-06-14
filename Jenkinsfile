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
               git branch: 'main', url: 'https://github.com/aksh153026/back-end.git'

            }
        }
     
        
        stage ('Compile Stage') {
            steps {
                
                bat "cd scrum-app && mvn -f pom.xml clean package"
                echo 'Build Compile Successful'
                }
            }
         
    
    }
	node("vagrant3"){
    
stage("com"){
    sh """#!/bin/bash
        rm -r scrum-ui
        git clone https://github.com/aksh153026/scrum-ui.git -b main
        """
}

}
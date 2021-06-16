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
       
    stage ('Push image to sonartype nexus') { // take that image and push to artifactory
        
         
    
        steps {
        
      script {
        docker.withRegistry('http://192.168.29.240:8083/', '1234') {

        bat "cd scrum-app && docker build -t 192.168.29.240:8083/backend:1.0.1 . && docker push 192.168.29.240:8083/backend:1.0.1"

       
    }
          
      }
         }
       
    }

}
        
    
}
node("vagrant1"){
    
    stage ('Push image to sonartype nexus') { // take that image and push to artifactory
        
     
        
      
        

        sh """#!/bin/bash 
        sudo docker login -u admin -p admin http://192.168.29.240:8083/
        sudo docker run -p 8080:8080 192.168.29.240:8083/backend:1.0.1 
        """

         }
       
    

    
}

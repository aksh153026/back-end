
pipeline {    
  agent {
      label 'master'
  }  
	
	/* options {
    skipDefaultCheckout true
  } */
    tools {
        git 'Default'
        nodejs 'NodeJS'
        maven 'MAVEN_HOME' 
        jdk 'jdk1.8' 
    }
  
  stages {
	  /*
	stage('Checkout SCM') {
            steps {
		    echo 'Hello World webhook'
       echo 'git repository name is :' + ref
		    echo 'git repository name is :' + base_ref
               checkout([$class: 'GitSCM', branches: [[name: 'main'], 
	       [name: 'dev'], [name: 'qa']],  doGenerateSubmoduleConfigurations: false, 
                          extensions: [],
                          gitTool: 'Default', userRemoteConfigs: [
                         [credentialsId: 'github',url: 'https://github.com/aksh153026/back-end.git']]])
                
            }
        }
        stage('GIT BRANCH NAME') {
            steps {
                
				script {
					 
					
					 env.GIT_BRANCH_PATH=sh(returnStdout: true, script: "git name-rev --name-only HEAD").trim()
					echo GIT_BRANCH_PATH 
    env.GIT_BRANCH_NAME=GIT_BRANCH_PATH.split('remotes/')[1]
     
				}
				
            }
        }
        stage('GIT BRANCH NAME1') {
            steps {
			echo GIT_BRANCH_NAME 
            }
        }
		
        stage ('Compile Stage back-end') {
            when {
				expression {   
					env.GIT_BRANCH_NAME=='origin/dev' 
				}
			}
			steps {
                
                bat "cd scrum-app && mvn -f pom.xml clean package"
                echo 'Build Compile Successful'
            }
		}
		
		*/
		stage ('Code Quality scan back-end')  {
			/* when {
				expression {   
					env.GIT_BRANCH_nNAME=='origin/qa' 
				}
			} */
			tools {
				jdk "jh" 
			}
    
			steps {
				script  {
					scannerHome = tool 'sonar_mvn'
					pom = readMavenPom file: 'scrum-app/pom.xml'
				echo pom.version
				
					// the name you have given the Sonar Scanner (Global Tool Configuration)
				}
    
				withSonarQubeEnv('sonar_mvn') {
         //   echo scannerHome 
					
					bat "cd scrum-app && mvn -f pom.xml clean package && "+scannerHome+"\\bin\\sonar-scanner.bat -Dsonar.projectKey=mavan -Dsonar.projectName=maven  -Dsonar.projectVersion=1.2 -Dsonar.sources=src/  -Dsonar.java.binaries=target/classes/* -Dsonar.java.source=1.8 -Dsonar.jacoco.reportsPath=target/jacoco.exec -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml -Dsonar.junit.reportsPath=target/surefire-reports/"
					
				}
				
			}
       }
	 
	    
		/*

        stage ('Push dev image to sonartype nexus back-end') { 
        
			when {
				expression {   
					env.GIT_BRANCH_NAME=='origin/dev' 
				}
			}
    
			steps {
        
				script {
					docker.withRegistry('http://192.168.29.240:8083/', '1234') {

					bat "cd scrum-app && docker build -t 192.168.29.240:8083/backend:${env.BUILD_ID} . && docker push 192.168.29.240:8083/backend:${env.BUILD_ID}"

       
					}
          
				}
			}
       
		}
	
		
		stage ('Push qa image to sonartype nexus back-end') { 
        
			when {
				expression {   
					env.GIT_BRANCH_NAME=='origin/qa' 
				}
			}
    
			steps {
        
				script {
					docker.withRegistry('http://192.168.29.240:8083/', '1234') {

					bat "cd scrum-app && docker build -t 192.168.29.240:8083/backend:${env.BUILD_ID} . && docker push 192.168.29.240:8083/backend:${env.BUILD_ID}"

       
					}
          
				}
			}
       
		}
	
		
		stage ('Push main image to sonartype nexus back-end') { 
        
			when {
				expression {   
					GIT_BRANCH_NAME == 'origin/main' 
				}
			}
    
			steps {
        
				script {
					docker.withRegistry('http://192.168.29.240:8083/', '1234') {

					bat "cd scrum-app && docker build -t 192.168.29.240:8083/backend:${env.BUILD_ID} . && docker push 192.168.29.240:8083/backend:${env.BUILD_ID}"

       
					}
          
				}
			}
       
		}
		
	
		stage ('Pull image from sonartype nexus in dev server back-end') { // take that image and push to artifactory
			when {
				expression {   
					env.GIT_BRANCH_NAME=='origin/dev' 
				}
			}
     
			agent {
				label 'v-test'
			}
      
			steps{   
				script {
				docker.withRegistry('http://192.168.29.240:8083/', '1234') {
				sh """#!/bin/bash 
				sudo docker login -u admin -p admin http://192.168.29.240:8083/
				sudo docker volume create scrum-data
				sudo docker container rm -f scrum-postgres
				sudo docker container rm -f scrum-app
				sudo docker run -dit -p 5432:5432 --name scrum-postgres -v scrum-data:/var/lib/postgresql/data -e POSTGRES_DB=scrum -e POSTGRES_USER=scrum -e POSTGRES_PASSWORD=scrum postgres:9.6-alpine
				sudo docker run -dit -p 8080:8080 --name scrum-app  -e DB_SERVER=scrum-postgres -e POSTGRES_DB=scrum -e POSTGRES_USER=scrum -e POSTGRES_PASSWORD=scrum --link scrum-postgres 192.168.29.240:8083/backend:${env.BUILD_ID}
				"""
				}
				} 
			}
    
          
		}
		
		
		
		stage ('Pull image from sonartype nexus in qa server back-end') { // take that image and push to artifactory
			when {
				expression {   
					env.GIT_BRANCH_NAME=='origin/qa' 
				}
			}
     
			agent {
				label 'v-test'
			}
      
			steps{   
				script {
				docker.withRegistry('http://192.168.29.240:8083/', '1234') {
				sh """#!/bin/bash 
				sudo docker login -u admin -p admin http://192.168.29.240:8083/
				sudo docker volume create scrum-data
				sudo docker container rm -f scrum-postgres
				sudo docker container rm -f scrum-app
				sudo docker run -d -p 5432:5432 --name scrum-postgres -v scrum-data:/var/lib/postgresql/data -e POSTGRES_DB=scrum -e POSTGRES_USER=scrum -e POSTGRES_PASSWORD=scrum postgres:9.6-alpine
				sudo docker run -d -p 8080:8080 --name scrum-app  -e DB_SERVER=scrum-postgres -e POSTGRES_DB=scrum -e POSTGRES_USER=scrum -e POSTGRES_PASSWORD=scrum --link scrum-postgres 192.168.29.240:8083/backend:${env.BUILD_ID}
				"""
				}
				}

			}
    
          
		}
		
		stage ('Pull image from sonartype nexus in main server back-end') { // take that image and push to artifactory
			when {
				expression {   
					env.GIT_BRANCH_NAME=='origin/main' 
				}
			}
     
			agent {
				label 'v-test'
			}
      
			steps
			{   
				script
				{
				docker.withRegistry('http://192.168.29.240:8083/', '1234') { 
				sh """#!/bin/bash 
				sudo docker login -u admin -p admin http://192.168.29.240:8083/
				sudo docker volume create scrum-data
				sudo docker container rm -f scrum-postgres
				sudo docker container rm -f scrum-app
				sudo docker run -d -p 5432:5432 --name scrum-postgres -v scrum-data:/var/lib/postgresql/data -e POSTGRES_DB=scrum -e POSTGRES_USER=scrum -e POSTGRES_PASSWORD=scrum postgres:9.6-alpine
				sudo docker run -d -p 8080:8080 --name scrum-app  -e DB_SERVER=scrum-postgres -e POSTGRES_DB=scrum -e POSTGRES_USER=scrum -e POSTGRES_PASSWORD=scrum --link scrum-postgres 192.168.29.240:8083/backend:${env.BUILD_ID} 
			
				"""
				}
			    }
			} 
				
          
		}
		
    }
     post {
        always {
            emailext body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
                recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
                subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}", to: 'aksh152630@gmail.com'
            
        }
    }*/
}

}

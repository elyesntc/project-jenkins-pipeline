pipeline {
    agent any
	
	  tools
    {
       maven "default"
    }
 stages {
      stage('checkout') {
           steps {
             
                git branch: 'main', url: 'https://github.com/elyesntc/project-jenkins-pipeline.git'
             
          }
        }
	 stage('Execute Maven') {
           steps {
             
                sh 'mvn package'             
          }
        }
        

  stage('Docker Build and Tag') {
           steps {
              
                sh 'docker build -t elyesproject:latest .' 
                sh 'docker tag elyesproject elyesntc/elyesproject:latest'
                //sh 'docker tag samplewebapp nikhilnidhi/samplewebapp:$BUILD_NUMBER'
               
          }
        }
     stage('Publish image to Docker Hub') {
          
            steps {
        withDockerRegistry([ credentialsId: "elyesntc", url: "https://hub.docker.com" ]) {
          sh  'docker push elyesntc/elyesproject:latest'
        //  sh  'docker push nikhilnidhi/samplewebapp:$BUILD_NUMBER' 
        }
      }
      }
      
       stage('Run Docker container on remote hosts') {
             steps {
                

                 
                   sh "docker -H ssh://elyes@192.168.1.7 run -d -p 8085:8086 --env DATABASE_HOST=mysql-standalone --env DATABASE_USER=sa --env DATABASE_PASSWORD=password --env DATABASE_NAME=test --env DATABASE_PORT=3306  elyesntc/elyesproject:latest"
                  
                 }
               }
             
            
	}
	}

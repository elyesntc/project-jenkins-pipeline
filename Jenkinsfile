pipeline {
    agent any
	
	  tools
    {
       maven "default"
    }
 stages {
      stage('checkout') {
           steps {
             
                git branch: 'master', url: 'https://github.com/AdnenSahly/project-jenkins-pipeline.git'
             
          }
        }
	 stage('Execute Maven') {
           steps {
             
                sh 'mvn package'             
          }
        }
        

  stage('Docker Build and Tag') {
           steps {
              
                sh 'docker build -t sahlyproject:latest .' 
                sh 'docker tag sahlyproject sahlyadnen/sahlyproject:latest'
                //sh 'docker tag samplewebapp nikhilnidhi/samplewebapp:$BUILD_NUMBER'
               
          }
        }
     stage('Publish image to Docker Hub') {
          
            steps {
        withDockerRegistry([ credentialsId: "dockerhubId", url: "" ]) {
          sh  'docker push sahlyadnen/sahlyproject:latest'
        //  sh  'docker push nikhilnidhi/samplewebapp:$BUILD_NUMBER' 
        }
      }
      }
      
       stage('Run Docker container on remote hosts') {
             steps {
                

                 
                   sh "docker -H ssh://sahly@192.168.45.156 run -d -p 8085:8086 --env DATABASE_HOST=mysql-standalone --env DATABASE_USER=sa --env DATABASE_PASSWORD=password --env DATABASE_NAME=test --env DATABASE_PORT=3306  sahlyadnen/sahlyproject:latest"
                  
                 }
               }
             
            
	}
	}
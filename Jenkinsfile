pipeline{
    agent any 
    environment{
        VERSION = "${env.BUILD_ID}"
	DOCKERHUB_CREDENTIALS = credentials('dockerhub')    
    }
    stages{
        stage("sonar quality check"){
         
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sona') {
                            sh 'chmod +x gradlew'
                            sh './gradlew sonarqube'
                    }

                }  
            }
        }
        stage("docker build"){
            steps{
                script{
	 sh '''
                docker build -t prahlad2083/chekk:$BUILD_NUMBER .
            '''
                    
                }
            }
        }
	stage("Docker Push"){

          steps{		
        withCredentials([string(credentialsId: 'pwddoc', variable: 'dockerhub')]) {
	 sh '''	
	docker login -u prahlad2083 -p ${dockerhub}	
        docker push prahlad2083/chekk:$BUILD_NUMBER
	 
         '''
	}
	}
	}    
           }
}

pipeline {

    agent any
/*
	
*/
    environment {
        registry = "swapnilsrd/vprofilecicd"
        registryCredential = 'dockerhubcreds'
    }

    stages{
 
	

        stage('Building image') {
            steps{
              script {
                dockerImage = docker.build registry + ":$BUILD_NUMBER"
              }
            }
        }
        stage('listing images') {
            steps{
               
              sh  'docker images'
              
            }
        }
        
       stage('Push Image to DockerHub') {
          steps{
            script {
              docker.withRegistry( 'https://hub.docker.com/', registryCredential) {
	      dockerImage.push(":$BUILD_NUMBER")
              dockerImage.push("latest")
              }
            }
          }
        }
        
        

    }


}

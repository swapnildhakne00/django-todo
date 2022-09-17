pipeline {

    agent any
/*
	
*/
    environment {
        registry = "swapnilsrd/vprofilecicd"
        registryCredential = 'dockerhub'
    }

    stages{
 
	

        stage('Building image') {
            steps{
              script {
                dockerImage = docker.build $registry + ":v$env.BUILD_NUMBER"
              }
            }
        }
        
        stage('Deploy Image') {
          steps{
            script {
              docker.withRegistry( '', registryCredential ) {
                dockerImage.push("v$env.BUILD_NUMBER")
                dockerImage.push('latest')
              }
            }
          }
        }

        stage('Remove Unused docker image') {
          steps{
            sh "docker rmi $registry:v$env.BUILD_NUMBER"
          }
        }

        
        

    }


}

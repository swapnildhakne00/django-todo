pipeline {

    agent any
/*
	
*/
    environment {
        registry = "swapnilsrd/vprofilecicd"
        registryCredential = 'dockerhub'
    }

    stages{
 
	stage('CODE ANALYSIS with SONARQUBE') {

            environment {
                scannerHome = tool 'mysonarscanner4'
            }

            steps {
                withSonarQubeEnv('sonarcloud') {
                    sh '''${scannerHome}/bin/sonar-scanner  \
                   -Dsonar.organization=swapnildhakne00 \
                   -Dsonar.projectKey=myfirstprojenkins \
                   -Dsonar.sources=. \
                   -Dsonar.host.url=https://sonarcloud.io'''
                }

                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Building image') {
            steps{
              script {
                dockerImage = docker.build registry + ":v$BUILD_NUMBER"
              }
            }
        }
        
        stage('Deploy Image') {
          steps{
            script {
              docker.withRegistry( '', registryCredential ) {
                dockerImage.push("v$BUILD_NUMBER")
                dockerImage.push('latest')
              }
            }
          }
        }

        stage('Remove Unused docker image') {
          steps{
            sh "docker rmi $registry:v$BUILD_NUMBER"
          }
        }

        
        

    }


}

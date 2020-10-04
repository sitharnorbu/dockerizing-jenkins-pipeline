# dockerizing-jenkins-pipeline
#
pipeline {
  environment {
    registry = "sitharnorbu/dockerizing-jenkins-pipeline"
    registryCredential = 'sithardockerhub'
  }
  agent any
  stages {
	stage('Cloning Git') {
	steps {
		git 'https://github.com/sitharnorbu/dockerizing-jenkins-pipeline.git'
	      }
	}

        stage('Building image') {
        steps{
            script {
            dockerImage = docker.build registry + ":$BUILD_NUMBER"
            }
        }
        }

        stage('Deploy Image') {
        steps{
            script {
            docker.withRegistry( '', 'dockerhub' ) {
                dockerImage.push()
            }
            }
        }
        }

        stage('Remove Image') {
        steps{
            sh "docker rmi $registry:$BUILD_NUMBER"
        }
        }
   }
}

node {
    stage('Execute Image'){
        def customImage = docker.build("sitharnorbu/dockerizing-jenkins-pipeline:${env.BUILD_NUMBER}")
        customImage.inside {
            sh 'echo This is the code executing inside the container.'
        }
    }
}

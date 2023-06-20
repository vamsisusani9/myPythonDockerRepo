pipeline {
    agent any
    environment {
        registry = '620911136265.dkr.ecr.ap-south-1.amazonaws.com/pythonapp'
    }
    stages {
        stage ('git checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: '2aa438d7-1332-470c-9474-f60d0b373406', url: 'https://github.com/KPhaniPrasad/myPythonDockerRepo.git']])
            }
        }
            
        stage ('Build docker image') {
          steps {
            script {
                dockerImage = docker.build registry
            }
          }  
        }
        stage ('Push Docker Image') {
            steps {
                script {
                    sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 620911136265.dkr.ecr.ap-south-1.amazonaws.com/pythonapp'
                    sh 'docker push 620911136265.dkr.ecr.ap-south-1.amazonaws.com/pythonapp:latest'
                }
            }
        }
      
    }
}

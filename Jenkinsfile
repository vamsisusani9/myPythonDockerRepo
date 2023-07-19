pipeline {
    agent any
    environment {
        registry = "343465709319.dkr.ecr.ap-south-1.amazonaws.com/demoapplication"
    }
    stages {
        stage ('Checkout stage') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/KPhaniPrasad/myPythonDockerRepo.git']])
            }
        }
        stage ('Building Docker Image') {
            steps{
                script {
                    dockerImage = docker.build registry
                
                }
            }   
        }
        stage ('Push to ECR') {
            steps {
                script {
                    sh 'sh aws ecr get-login-password-region ap-south-1| docker login --username AWS --password-stdin 343465709319.dkr.ecr.ap-south-1.amazonaws.com/demoapplication'
                    sh 'sh docker push 343465709319.dkr.ecr.ap-south-1.amazonaws.com/demoapplication:120'

                    
                }
            }
        }
        stage ('Deploy to EKS') {
            steps {
                script {
                    kubernetesDeploy (
                        configs: 'k8s-deployment.yaml',
                        kubeconfigId: 'K8S',
                        enableConfigSubstitution: true
                    )
                }
            }
        }
        
    }
}

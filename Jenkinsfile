
pipeline {
   tools {
        maven 'Maven3'
    }
    agent any
    environment {
        registry = "150772578985.dkr.ecr.eu-west-2.amazonaws.com/k8s-project"
    }
   
    stages {
        stage('Checkout Github') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/WAZEEF/Kubernetes_Project.git']])    
            }
        }
      stage ('Build Maven') {
          steps {
            sh 'mvn clean package'           
            }
      }
    stage ('Docker Build') {
      steps{
        sh 'docker build -t 150772578985.dkr.ecr.eu-west-2.amazonaws.com/k8s-project:latest .'
      }
    }
    stage('Pushing to ECR') {
     steps{  
                sh 'aws ecr get-login-password --region eu-west-2 | docker login --username AWS --password-stdin 150772578985.dkr.ecr.eu-west-2.amazonaws.com'
                sh 'docker push 150772578985.dkr.ecr.eu-west-2.amazonaws.com/k8s-project'
         }
        }
       stage('K8S Deploy') {
            steps{
                sh ('kubectl apply -f  eks-deploy-k8s.yaml')           
          }
    }
}
}

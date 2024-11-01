pipeline {
    agent any

    environment {
        registry = "329599628577.dkr.ecr.us-west-2.amazonaws.com/tumidola/helmjenkinsk8s"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/tumidola/docker-spring-boot']])
            }
        }
        
        stage ("Build JAR") {
            steps {
                sh "mvn clean install"
            }
        }
        
        stage ("Build Image") {
            steps {
                script {
                    docker.build registry
                }
            }
        }
        
        stage ("Push to ECR") {
            steps {
                script {
                    sh "aws ecr get-login-password --region us-west-1 | docker login --username AWS --password-stdin 329599628577.dkr.ecr.us-west-2.amazonaws.com"
                    sh "docker push 329599628577.dkr.ecr.us-west-2.amazonaws.com/tumidola/helmjenkinsk8s"
                    
                }
            }
        }
        
        stage ("Helm package") {
            steps {
                    sh "helm package springboot"
                }
            }
                
        stage ("Helm install") {
            steps {
                    sh "helm upgrade myrelease-21 springboot-0.1.0.tgz"
                }
            }
    }
}

pipeline {
    agent any
    tools {
        maven 'maven_3_5_0'
    }

     stages {
        stage('Build Maven') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Abhigit90/devops-automation.git']])
                sh 'mvn clean install'
            }
        }
        stage('Docker build image'){
            steps{
                script{
                    sh 'docker rm -f $(docker ps -aq)'
                    sh 'docker rmi -f $(docker images -aq)'
                    sh 'docker build -t abhishek9888/devops-intergation .'
                }
            }
        }
        stage('docker container creation'){
            steps{
                script{
                    sh 'docker run -itd --name con1 -p 8081:8080 abhishek9888/devops-intergation'
                }
            }
        }
        stage('image push dockerhub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhub')]) {
                    sh 'docker login -u abhishek9888 -p ${dockerhub}'
                    }
                    sh 'docker push abhishek9888/devops-intergation'
                }
            }
        }
     }
}

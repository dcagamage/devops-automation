pipeline{
    agent any
    tools{
        maven 'maven_3_9_10'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/dcagamage/devops-automation']])
                bat 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    bat 'docker build -t dcagamage/devops-integration.jar .'
                }
            } 
        }
        stage('Push image to hub'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', 
                                          usernameVariable: 'DOCKERHUB_USER', 
                                          passwordVariable: 'DOCKERHUB_PWD')]) {
                    bat """
                        echo %DOCKERHUB_PWD% | docker login -u %DOCKERHUB_USER% --password-stdin
                        docker tag dcagamage/devops-integration.jar:latest dcagamage/devops-integration:latest
                        docker push dcagamage/devops-integration:latest
                    """
                }
            }
        }
    }
}
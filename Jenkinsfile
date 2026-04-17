pipeline {
    agent any

    stages {
        stage('Build Maven') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/dharjhagithub/devops-repo'
                    ]]
                ])

                script {
                    def mvnHome = tool 'Maven_3'
                    sh "${mvnHome}/bin/mvn clean verify"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker version'
               sh 'docker build -t dharjha/devops-integration .'
            }
        }
        
        
 stage('Login & Push Image to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login \
                          -u "$DOCKER_USER" \
                          --password-stdin
                    '''
                    sh 'docker push dharjha/devops-integration:latest'
                }
            }
        }
    }
}
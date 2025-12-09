pipeline {
    agent any

    environment {
        DOCKERHUB_USER = "sky91win"
        DOCKERHUB_REPO = "project9"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/sky91win/project9.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $DOCKERHUB_USER/$DOCKERHUB_REPO:latest .
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DUSER',
                    passwordVariable: 'DPASS'
                )]) {
                    sh '''
                    docker login -u "$DUSER" -p "$DPASS"
                    docker push $DOCKERHUB_USER/$DOCKERHUB_REPO:latest
                    '''
                }
            }
        }

        stage('Deploy Using Ansible') {
            steps {
                sh 'ansible-playbook -i inventory deploy.yml'
            }
        }
    }
}

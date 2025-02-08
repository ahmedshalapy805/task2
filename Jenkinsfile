pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'ahmedshalapy'
        DOCKERHUB_PASS = '11115555Aa'
        DOCKER_IMAGE = 'ahmedshalapy/final_project'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/Omareldeeb7/Final_Project.git'
            }
        }

        stage('Build and Test') {
            steps {
                sh 'chmod +x mvnw'
                sh './mvnw clean package'
                sh './mvnw test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-credentials', url: '']) {
                    sh 'docker login -u $DOCKERHUB_USER -p $DOCKERHUB_PASS'
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy with Ansible') {
            steps {
                sh 'ansible-playbook -i ansible/inventory ansible/playbook.yml'
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
    }
}

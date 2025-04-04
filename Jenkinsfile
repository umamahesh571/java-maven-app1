def gv

pipeline {
    agent any
    
    environment {
        REPO_URL = 'https://github.com/umamahesh571/java-maven-app1.git'
        BRANCH = 'main'
        IMAGE_NAME = 'java-maven-app1'
        CONTAINER_NAME = 'java-maven-app1-container'
        PORT = '8081'
    }
    
    stages {
        stage('Checkout Code') {
            steps {
                script {
                    checkout([
                        $class: 'GitSCM', 
                        branches: [[name: "*/${BRANCH}"]], 
                        userRemoteConfigs: [[url: REPO_URL]]
                    ])
                }
            }
        }
        
        stage('Build') {
            steps {
                script {
                    sh 'mvn clean package'
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    sh '''
                        cp target/*.jar app.jar
                        docker build -t ${IMAGE_NAME} .
                    '''
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    sh '''
                        docker stop ${CONTAINER_NAME} || true
                        docker rm ${CONTAINER_NAME} || true
                        docker run -d --name ${CONTAINER_NAME} -p ${PORT}:8080 ${IMAGE_NAME}
                    '''
                }
            }
        }
    }
}


pipeline {
    agent any
    
    environment {
        SONAR_HOME =tool 'QubeScanner'
    }
    
    tools {
        maven 'Maven-3.8.8'
        jdk 'JDK-17'
        nodejs 'Node-16'
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo "Checkout repo"
                git branch:'main', url: 'https://github.com/NQTin/DevSecOps-Pipeline-Project.git'
            }
        }
        stage('SonarQube analyse source code') {
            steps {
                withSonarQubeEnv('SonarQube-Server') {
                    sh ''' $SONAR_HOME/bin/sonar-scanner \
                    -Dsonar.projectName=Utube \
                    -Dsonar.projectKey=Utube 
                    '''
                }
            }
        }
        stage ('Quality gate check') {
            steps {
                waitForQualityGate abortPipeline: true, credentialsId: 'SonarQubeAccessToken'
            }
        }
        stage ('Trivy scan') {
            steps {
                sh "trivy fs . > trivyfs.txt"
            }
        }
        stage ('Build image') {
            environment {
                API_KEY = credentials('Utube_API_KEY')
                ImageName = 'tinnqforwork/utubecicd'
                Version = "${BUILD_NUMBER}"
            }
            steps {
                script {
                    env.DOCKER_IMAGE = "$ImageName:v$Version"
                }
                sh '''
                    docker build --build-arg REACT_APP_RAPID_API_KEY=$API_KEY -t $DOCKER_IMAGE . 
                '''
            }
        }
        stage ('Push image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DockerHubToken', usernameVariable:'DOCKER_USER', passwordVariable:'DOCKER_PASSD')]) {
                    sh '''
                        echo $DOCKER_PASSD | docker login -u $DOCKER_USER --password-stdin
                        docker push $DOCKER_IMAGE
                    '''
                }
            }
        }
        stage ('Deploy app') {
            environment {
                CONTAINER_NAME = 'Utube-app'
            }
            steps {
                script {
                    sh "docker pull DOCKER_IMAGE"
                    sh "docker rm -f $CONTAINER_NAME || true"
                    sh "docker run --name $CONTAINER_NAME -dp 3000:3000 DOCKER_IMAGE"
                }
            }
        }
    }
}

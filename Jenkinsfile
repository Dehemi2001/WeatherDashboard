pipeline {
    agent any

    environment {
        IMAGE_NAME = 'weatherdashboard'
        CONTAINER_NAME = 'weatherdashboard'
        DOTNET_PROJECT = 'WeatherDashboard/WeatherDashboard.csproj'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'dotnet build ${DOTNET_PROJECT} --configuration Release'
            }
        }
        stage('Test') {
            steps {
                sh 'dotnet test WeatherDashboard.Tests/WeatherDashboard.Tests.csproj --no-build --verbosity normal'
            }
        }
        stage('Publish') {
            steps {
                sh 'dotnet publish ${DOTNET_PROJECT} --configuration Release --output publish'
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build -t ${IMAGE_NAME} .'
            }
        }
        stage('Docker Run') {
            steps {
                // Stop and remove any existing container
                sh '''
                    docker rm -f ${CONTAINER_NAME} || true
                    docker run -d --name ${CONTAINER_NAME} -p 80:80 ${IMAGE_NAME}
                '''
            }
        }
    }
    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
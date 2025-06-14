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
                bat 'dotnet build %DOTNET_PROJECT% --configuration Release'
            }
        }
        stage('Test') {
            steps {
                bat 'dotnet test WeatherDashboard.Tests/WeatherDashboard.Tests.csproj --configuration Release --verbosity normal'
            }
        }
        stage('Publish') {
            steps {
                bat 'dotnet publish %DOTNET_PROJECT% --configuration Release --output publish'
            }
        }
        stage('Docker Build') {
            steps {
                bat 'docker build -t %IMAGE_NAME% .'
            }
        }
        stage('Docker Run') {
            steps {
                // Stop and remove any existing container
                bat '''
                    docker rm -f %CONTAINER_NAME%
                    docker run -d --name %CONTAINER_NAME% -p 80:80 %IMAGE_NAME%
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
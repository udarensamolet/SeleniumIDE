pipeline {
    agent any

    environment {
        CHROMEWEBDRIVER = 'C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe'
    }

    stages {
        stage('Checkout code') {
            steps {
                // Checkout code from GitHub and specify the branch
                git branch: 'main', url: 'https://github.com/udarensamolet/SeleniumIDE.git'
            }
        }

        stage('Set up .NET Core') {
            steps {
                bat '''
                echo Installing .NET SDK 6.0
                choco install dotnet-sdk -y --version=6.0.100
                '''
            }
        }

        stage('Install Chrome') {
            steps {
                bat '''
                echo Installing Google Chrome
                choco install googlechrome -y
                '''
            }
        }

        stage('Restore dependencies') {
            steps {
                bat 'dotnet restore HouseRentingSystem.sln'
            }
        }

        stage('Build') {
            steps {
                bat 'dotnet build HouseRentingSystem.sln --configuration Release'
            }
        }

        stage('Run tests') {
            steps {
                bat 'dotnet test HouseRentingSystem.Tests/HouseRentingSystem.Tests.csproj'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/TestResults/*.xml', allowEmptyArchive: true
            junit '**/TestResults/*.xml'
        }
    }
}

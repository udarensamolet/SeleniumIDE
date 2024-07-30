pipeline {
    agent any

    environment {
        CHROMEWEBDRIVER = '/usr/bin/google-chrome'
    }

    stages {
        stage('Checkout code') {
            steps {
                // Checkout code from GitHub
                git 'https://github.com/udarensamolet/SeleniumIDE.git'
            }
        }

        stage('Set up .NET Core') {
            steps {
                sh 'sudo apt-get update'
                sh 'sudo apt-get install -y dotnet-sdk-6.0'
            }
        }

        stage('Install Chrome') {
            steps {
                sh '''
                sudo apt-get update
                sudo apt-get install -y google-chrome-stable
                '''
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build --no-restore'
            }
        }

        stage('Run tests') {
            steps {
                sh 'dotnet test --verbosity normal'
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

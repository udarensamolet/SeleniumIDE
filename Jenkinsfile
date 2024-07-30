pipeline {
    agent any

    environment {
        CHROMEWEBDRIVER = '/usr/bin/google-chrome'
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
                sh '''
                set -e
                echo "Updating package lists"
                sudo apt-get update

                echo "Installing wget"
                sudo apt-get install -y wget

                echo "Downloading Microsoft package"
                wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb

                echo "Installing Microsoft package"
                sudo dpkg -i packages-microsoft-prod.deb

                echo "Updating package lists again"
                sudo apt-get update

                echo "Installing apt-transport-https"
                sudo apt-get install -y apt-transport-https

                echo "Updating package lists one more time"
                sudo apt-get update

                echo "Installing .NET SDK 6.0"
                sudo apt-get install -y dotnet-sdk-6.0
                '''
            }
        }

        stage('Install Chrome') {
            steps {
                sh '''
                set -e
                echo "Updating package lists"
                sudo apt-get update

                echo "Installing Google Chrome"
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

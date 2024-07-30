pipeline {
    agent any

    environment {
        CHROMEWEBDRIVER = 'C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe'
        CHROME_VERSION = '127.0.6533.73' // Your Chrome version
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

        stage('Download ChromeDriver') {
            steps {
                bat '''
                echo Downloading ChromeDriver for Chrome version %CHROME_VERSION%
                powershell -command "Invoke-WebRequest -Uri https://chromedriver.storage.googleapis.com/%CHROME_VERSION%/chromedriver_win32.zip -OutFile chromedriver.zip"
                powershell -command "Expand-Archive chromedriver.zip -DestinationPath ."
                powershell -command "Move-Item -Path .\\chromedriver.exe -Destination C:\\Program Files\\Google\\Chrome\\Application\\chromedriver.exe -Force"
                '''
            }
        }

        stage('Restore dependencies') {
            steps {
                // Restore dependencies using the solution file
                bat 'dotnet restore SeleniumIde.sln'
            }
        }

        stage('Build') {
            steps {
                // Build the project using the solution file
                bat 'dotnet build SeleniumIde.sln --configuration Release'
            }
        }

        stage('Run tests') {
            steps {
                // Run tests using the solution file
                bat 'dotnet test SeleniumIde.sln --logger "trx;LogFileName=TestResults.trx"'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
            junit '**/TestResults/*.trx'
        }
    }
}

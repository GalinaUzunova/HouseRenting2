pipeline {
    agent any
    
    environment {
        DOTNET_CLI_TELEMETRY_OPTOUT = '1'  // Opt out of .NET CLI telemetry
    }
    
    stages {
        stage('RESTORE') {
            steps {
                script {
                    echo 'Restoring NuGet packages...'
                    bat 'dotnet restore'  // For Windows agents
                    // If using Linux agents, use: sh 'dotnet restore'
                }
            }
        }
        
        stage('BUILD') {
            steps {
                script {
                    echo 'Building project...'
                    bat 'dotnet build --configuration Release --no-restore'
                    // For Linux: sh 'dotnet build --configuration Release --no-restore'
                }
            }
        }
        
        stage('TEST') {
            steps {
                script {
                    echo 'Running tests...'
                    bat 'dotnet test --configuration Release --no-build --logger "trx;LogFileName=testresults.trx"'
                    // For Linux: sh 'dotnet test --configuration Release --no-build --logger "trx;LogFileName=testresults.trx"'
                }
            }
            post {
                always {
                    // Publish test results
                    publishHTML(target: [
                        allowMissing: false,
                        alwaysLinkToLastBuild: false,
                        keepAll: true,
                        reportDir: 'TestResults',
                        reportFiles: 'testresults.trx',
                        reportName: 'Test Results'
                    ])
                }
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline completed.'
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
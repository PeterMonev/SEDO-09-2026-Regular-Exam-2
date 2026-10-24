pipeline {
    agent any

    options {
        disableConcurrentBuilds()
        skipDefaultCheckout(true)
        timeout(time: 20, unit: 'MINUTES')
    }

    triggers {
        pollSCM('H/2 * * * *')
    }

    environment {
        DOTNET_NOLOGO = 'true'
        DOTNET_CLI_TELEMETRY_OPTOUT = '1'
    }

    stages {
        stage('Checkout main') {
            steps {
                checkout scm
            }
        }

        stage('Restore') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'dotnet restore Homies.sln'
                    } else {
                        bat 'dotnet restore Homies.sln'
                    }
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'dotnet build Homies.sln --configuration Release --no-restore'
                    } else {
                        bat 'dotnet build Homies.sln --configuration Release --no-restore'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'dotnet test Homies.sln --configuration Release --no-build --no-restore --verbosity normal'
                    } else {
                        bat 'dotnet test Homies.sln --configuration Release --no-build --no-restore --verbosity normal'
                    }
                }
            }
        }
    }
}

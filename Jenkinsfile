pipeline {
    agent any
    tools {
        nodejs 'nodejs-19.9.0'
    }
    environment {
        registry = "zentech.jfrog.io/docker/nodejs-app:${BUILD_NUMBER}"
        dockerImage = ""
        artifactory_Creden = credentials('artifactory-credentials')
    }
    stages {
        stage('git checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'Github-Credentials', url: 'https://github.com/narendra7306/nodejs-app.git']])
            }
        }
        stage('SonarQube analysis') {
            environment {
                scannerHome = tool 'SonarScanner'
                PROJECT_NAME = "sr-nodejs-app"
            }
            steps {
                withSonarQubeEnv('SonarQubeServer') { // If you have configured more than one global server connection, you can specify its name
                  sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=$PROJECT_NAME \
                    -Dsonar.sources=."
                }
            }
        }
        stage('nodejs build') {
            steps {
                sh 'npm install'
            }
        }
        stage('docker build') {
            steps {
                script {
                    dockerImage = docker.build registry
                }
            }
        }
        stage('Image push') {
            steps {
                sh 'docker login -u $artifactory_Creden_USR -p $artifactory_Creden_PSW zentech.jfrog.io'
                sh 'docker push ${registry}'
            }
        }
    }
}

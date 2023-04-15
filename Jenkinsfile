pipeline {
    agent any
    tools {
        nodejs 'nodejs-19.9.0'
    }
    stages {
        stage('git checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'Github-Credentials', url: 'https://github.com/narendra7306/nodejs-app.git']])
            }
        }
        stage('nodejs build') {
            steps {
                sh 'npm install'
            }
        }
    }
}

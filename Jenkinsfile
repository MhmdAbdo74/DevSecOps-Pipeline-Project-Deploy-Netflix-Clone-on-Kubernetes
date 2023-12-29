pipeline {
    agent any
    tools {
        jdk 'jdk'
        nodejs 'nodejs'
    }
    environment {
        SCANNER_HOME = tool 'sonarscanner'
    }
    stages {
             stage('clean workspace') {
            steps {
                cleanWs()
            }
        }
              stage('Checkout from Git') {
            steps {
                git branch: 'main', url: 'https://github.com/MhmdAbdo74/DevSecOps-Pipeline-Project-Deploy-Netflix-Clone-on-Kubernetes.git'
            }
        }
              stage("Sonarqube Analysis") {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Netflix \
                    -Dsonar.projectKey=Netflix'''
                }
        
    }
}
    }
}
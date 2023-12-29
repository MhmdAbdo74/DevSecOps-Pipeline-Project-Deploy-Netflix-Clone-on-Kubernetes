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
        
    }
}
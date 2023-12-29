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
                withSonarQubeEnv('sonarserver') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Netflix \
                    -Dsonar.projectKey=Netflix'''
                }
            }
              }
                stage("quality gate") {
            steps {
                    timeout(time: 1, unit: 'HOURS'){
                    waitForQualityGate abortPipeline: true
                    }
                }
            }
    
              stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        stage('OWASP FS SCAN') {
            steps {
                dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', odcInstallation: 'DP-Check'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
            }
             stage('TRIVY FS SCAN') {
            steps {
                sh "trivy fs . > trivyfs.txt"
            }
        }
        stage("Docker Build & Push"){
            steps{
                script{
                   withDockerRegistry(credentialsId: 'docker_cred', toolName: 'docker'){   
                       sh "docker build --build-arg TMDB_V3_API_KEY=${API_Key} -t netflix ."
                       sh "docker tag netflix mohamed222/netflix:latest "
                       sh "docker push mohamed222/netflix:latest "
                    }
                }
            }
        }
         stage('Deploy to container'){
            steps{
                sh 'docker run -d --name netflix -p 8081:80 mohamed222/netflix:latest'
            }
        }

    
}
}
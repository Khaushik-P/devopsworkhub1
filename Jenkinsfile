pipeline {
    agent any
    parameters{
        choice(name:'action' , choices:'create\ndelete' ,description:'Select create or destroy.')
    }
    
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }
    
    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
        
        stage('Git Checkout') {
            steps {
                git branch: 'main', credentialsId: 'f22fbe83-b5d3-4455-b4b2-84ae8952dc4c', url: 'https://github.com/Khaushik-P/DevopsWorkHub.git'
            }
        }
        
        stage("Sonarqube Analysis") {
            when { expression { params.action == 'create'}}  
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh "$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=DevopsWorkHub -Dsonar.projectKey=DevopsWorkHub"
                }
            }
        }
         stage('TRIVY FS SCAN') {
            when { expression { params.action == 'create'}}
            steps {
                sh "trivy fs . > trivyfs.txt"
            }
        }
            stage('Check Docker Compose') {
            when { expression { params.action == 'create'}}  
            steps {
                script {
                    def dockerComposeFile = fileExists('docker-compose.yml')
                    if (dockerComposeFile) {
                        echo 'Docker Compose file found.'
                        // Run docker-compose up
                        sh 'docker-compose up -d'
                    } else {
                        echo 'Docker Compose file not found.'
                    }
                }
            }
        }
        
        stage('Delete Docker Image'){
            when { expression { params.action == 'delete'}}  
            steps{
                script{
                    sh "docker-compose down"
                }
            }
        }
       
    }
}

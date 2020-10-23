pipeline {
    agent none
    stages {
        stage('build') {
            agent {
                docker { image 'gradle' }
            }
            steps {
                sh 'chmod +x gradlew && ./gradlew build'
            }
        }
        stage('sonarqube') { 
            agent {
                docker { image 'busybox' }
            }
            environment {
                SCANNER_HOME = tool 'SonarQubeScanner'
                ORGANIZATION = "fox10147"
                PROJECT_NAME = "Pre-Interview"
            }
            steps {
                sh 'echo sonarqube'
                withSonarQubeEnv(installationName: 'sonarcloud', credentialsId: '91b9a37a-8b4c-4585-aa1a-652c4a68768b') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner \
                    -Dsonar.organization=$ORGANIZATION \
                    -Dsonar.java.binaries=build/classes/java/ \
                    -Dsonar.projectKey=$PROJECT_NAME \
                    -Dsonar.sources=.'''
            }
        }
        }
        stage('docker build') {
            agent {
                docker { image 'busybox' }
            }
            steps {
                sh 'echo docker build'
            }
        }
        stage('docker push') {
            agent {
                docker { image 'busybox' }
            }
            steps {
                sh 'echo docker push'
            }
        }
        stage('app deploy') {
            agent {
                docker { image 'busybox' }
            }
            steps {
                sh 'echo kube deploy'
            }
        }
    }
}
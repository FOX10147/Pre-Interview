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
                withSonarQubeEnv(installationName: 'sonarcloud', credentialsId: 'Sonar Token') {
                    sh '''$SCANNER_HOME/bin/soar-scanner \
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
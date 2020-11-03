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
                docker { image 'gradle' }
            }
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh 'echo sonarqube'
                    sh './gradlew sonarqube'
                }
                sh "echo ${env.SONAR_HOST_URL}"
            }
        }
        stage('docker build') {
            agent {
                docker { image 'docker' }
            }
            steps {
                sh "echo docker build"
                sh "docker build -t fox10147/spring-boot:${env.BUILD_NUMBER} ."
                sh "docker tag fox10147/spring-boot:${env.BUILD_NUMBER} fox10147/spring-boot:latest"
            }
        }
        stage('docker push') {
            agent {
                docker { image 'docker' }
            }
            steps {
                sh "echo docker push"
                withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                    sh "docker push fox10147/spring-boot:${env.BUILD_NUMBER}"
                    sh "docker push fox10147/spring-boot:latest"
                }
            }
        }
        stage('app deploy') {
            agent {
                docker { image 'fox10147/kubecli' }
            }
            steps {
                withKubeConfig([credentialsId: 'kube-creds', serverUrl: 'https://kubernetes.docker.internal:6443']) {
                    sh 'kubectl apply -f kubernetes.yml'
                }
            }
        }
    }
}
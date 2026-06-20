pipeline {
    agent {
        label 'ms'
    }
    stages {
        stage('git clone') {
            steps {
                git branch: 'main', url: 'https://github.com/devsecopstrainer/student-service.git'
            }
        }
        stage('Mvn-Pkg') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Docker-Build') {
            steps {
                sh 'pwd'
                sh 'ls -ltr'
                sh 'echo ${BUILD_NUMBER}'
                sh "docker build -t student-service:${BUILD_NUMBER} ."
            }
        }
    }
}

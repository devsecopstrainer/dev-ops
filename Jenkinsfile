pipeline {
    agent {
        label 'ms'
    }
    parameters {
        string(name: 'IMAGE_VERSION', defaultValue: '1.0.0', description: 'Docker image version')
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
                sh "docker build -t student-service:${params.IMAGE_VERSION} ."
            }
        }
    }
}

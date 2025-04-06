pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                    sh 'go build .'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    sh 'go test -v'
                }
            }
        }
        stage('Docker Build') {
            steps {
                script {
                    def appName = "my-first-git-repo"
                    def imageName = "dmitriy-py/${appName}"
                    sh "docker build -t ${imageName} ."
                    sh "docker tag ${imageName} dmitriy-py/${appName}:latest"
                    withCredentials([usernamePassword(credentialsId: 'my-dockerhub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh "docker login -u ${USERNAME} -p ${PASSWORD}"
                        sh "docker push dmitriy-py/${appName}:latest"
                    }
                }
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

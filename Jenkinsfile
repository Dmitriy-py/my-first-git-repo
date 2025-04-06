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
                    def imageName = "dikdemon/${appName}"
                    sh "docker build -t ${imageName} ."
                    sh "docker tag ${imageName} dikdemon/${appName}:latest"
                    withCredentials([usernamePassword(credentialsId: 'my-dockerhub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh "docker login -u ${USERNAME} -p ${PASSWORD}"
                        sh "docker push dikdemon/${appName}:latest"
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

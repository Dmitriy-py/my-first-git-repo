pipeline {
    agent { docker { image 'golang:1.22-alpine' } }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Dmitriy-py/my-first-git-repo.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                sh 'go build .'
            }
        }
        stage('Test') {
            steps {
                sh 'go test -v'
            }
        }
        stage('Docker Build') {
            agent { docker { image: 'docker:latest', args: '-u root:root' } }
            steps {
                script {
                    def appName = "my-first-git-repo"
                    def imageName = "dmitriy-py/${appName}"
                    sh "docker build -t ${imageName} ."
                    sh "docker tag ${imageName} dmitriy-py/${appName}:latest"
                    withCredentials([usernamePassword(credentialsId: 'my-dockerhub-cred', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
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

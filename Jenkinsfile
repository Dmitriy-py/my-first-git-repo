pipeline {
    agent any
    stages {
        stage('Checkout') { // Я добавил этот stage, чтобы явно была видна операция checkout
            steps {
                git url: 'https://github.com/Dmitriy-py/my-first-git-repo.git', branch: 'main' // Замените на URL вашего репозитория
            }
        }
        stage('Build') {
            steps {
                sh 'go build -o myapp .' // Сборка Go-файла с именем "myapp"
            }
        }
       stage('Upload to Nexus') {
            agent any
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    nexusUrl: 'http://localhost:8081/', // Замените на URL вашего Nexus
                    nexusRepositoryId: 'go-binaries',  // Замените на имя вашего raw-hosted репозитория
                    groupId: 'com.example',       // Замените на ваш groupId (можно использовать любой)
                    version: '1.0',               // Замените на вашу версию (можно использовать любую)
                    artifactId: 'myapp',          // Имя вашего артефакта (совпадает с именем бинарника)
                    classifier: '',
                    artifact: 'myapp',             // Путь к бинарному файлу, относительно workspace Jenkins
                    credentialsId: 'nexus-creds' // ID учетных данных Nexus, которые вы создали
                )
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

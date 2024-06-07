pipeline {
    agent any

    environment {
        MAVEN = tool name: 'Maven 3.9.6', type: 'maven'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Blisstod/DemoSite.git'  // Убедитесь, что URL репозитория правильный
            }
        }
        stage('Build') {
            steps {
                sh "${MAVEN}/bin/mvn clean install"  // Сборка проекта
            }
        }
        stage('Test') {
            steps {
                sh "${MAVEN}/bin/mvn test"  // Запуск тестов
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def services = ['site', 'admin', 'api']
                    for (service in services) {
                        sh "cd ${service} && ${MAVEN}/bin/mvn spring-boot:run"  // Запуск сервисов
                    }
                }
            }
        }
    }

    post {
        success {
            mail to: 'onetenge@gmail.com',
                subject: "Successful Deployment: ${currentBuild.fullDisplayName}",
                body: "The deployment was successful."  // Уведомление об успешной сборке
        }
        failure {
            mail to: 'onetenge@gmail.com',
                subject: "Failed Deployment: ${currentBuild.fullDisplayName}",
                body: "The deployment failed. Please check the Jenkins logs for more details."  // Уведомление об ошибке сборки
        }
    }
}

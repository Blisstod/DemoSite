pipeline {
    agent any

    environment {
        MAVEN_HOME = tool name: 'Maven 3.6.3', type: 'maven'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Blisstod/DemoSite.git'  // Убедитесь, что URL репозитория правильный
            }
        }
        stage('Build') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn clean install"  // Сборка проекта
            }
        }
        stage('Test') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn test"  // Запуск тестов
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def services = ['site', 'admin', 'api']
                    for (service in services) {
                        sh "cd ${service} && ${MAVEN_HOME}/bin/mvn spring-boot:run"  // Запуск сервисов
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

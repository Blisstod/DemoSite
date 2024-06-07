pipeline {
    agent any

    environment {
        MAVEN_HOME = tool name: 'Maven 3.9.6', type: 'maven'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Blisstod/DemoSite.git'  // Убедитесь, что URL репозитория правильный
            }
        }
        stage('Build') {
            steps {
                bat "${MAVEN_HOME}\\bin\\mvn clean install"  // Сборка проекта
            }
        }
        stage('Test') {
            steps {
                bat "${MAVEN_HOME}\\bin\\mvn test"  // Запуск тестов
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def services = ['site', 'admin', 'api']
                    for (service in services) {
                        bat "cd ${service} && ${MAVEN_HOME}\\bin\\mvn spring-boot:run"  // Запуск сервисов
                    }
                }
            }
        }
    }

    post {
        success {
            script {
                echo "Build was successful!"
                // mail to: 'onetenge@gmail.com',
                //     subject: "Successful Deployment: ${currentBuild.fullDisplayName}",
                //     body: "The deployment was successful."  // Уведомление об успешной сборке
            }
        }
        failure {
            script {
                echo "Build failed!"
                // mail to: 'onetenge@gmail.com',
                //     subject: "Failed Deployment: ${currentBuild.fullDisplayName}",
                //     body: "The deployment failed. Please check the Jenkins logs for more details."  // Уведомление об ошибке сборки
            }
        }
    }
}

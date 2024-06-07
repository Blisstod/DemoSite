pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Blisstod/DemoSite.git'  // Убедитесь, что URL репозитория правильный
            }
        }
        stage('Build') {
            steps {
                script {
                    def mavenHome = tool name: 'Maven 3.9.6', type: 'maven'
                    sh "${mavenHome}/bin/mvn clean install -X"
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    def mavenHome = tool name: 'Maven 3.9.6', type: 'maven'
                    sh "${mavenHome}/bin/mvn test"
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def mavenHome = tool name: 'Maven 3.9.6', type: 'maven'
                    def services = ['site', 'admin', 'api']
                    for (service in services) {
                        bat "cd ${service} && ${mavenHome}\\bin\\mvn spring-boot:run"  // Запуск сервисов
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

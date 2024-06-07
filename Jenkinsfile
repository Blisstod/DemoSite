pipeline {
    agent any

    environment {
        MAVEN_OPTS = "-Xmx1024m"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Blisstod/DemoSite.git'  // Убедитесь, что URL репозитория правильный
            }
        }
        stage('Build') {
            steps {
                script {
                    echo 'Building the project...'
                    bat 'mvn clean install -X'  // Сборка проекта с использованием Maven
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                    bat 'mvn test'  // Запуск тестов с использованием Maven
                }
            }
        }
        stage('Deploy Site') {
            steps {
                script {
                    echo 'Deploying Site...'
                    dir('site') {
                        bat 'mvn spring-boot:run'  // Запуск сервиса site с использованием Maven
                    }
                }
            }
        }
        stage('Deploy Admin') {
            steps {
                script {
                    echo 'Deploying Admin...'
                    dir('admin') {
                        bat 'mvn spring-boot:run'  // Запуск сервиса admin с использованием Maven
                    }
                }
            }
        }
        stage('Deploy API') {
            steps {
                script {
                    echo 'Deploying API...'
                    dir('api') {
                        bat 'mvn spring-boot:run'  // Запуск сервиса api с использованием Maven
                    }
                }
            }
        }
    }

    post {
        success {
            script {
                echo "Build was successful!"
                // Uncomment the following lines to send an email notification on success
                // mail to: 'onetenge@gmail.com',
                //     subject: "Successful Deployment: ${currentBuild.fullDisplayName}",
                //     body: "The deployment was successful."
            }
        }
        failure {
            script {
                echo "Build failed!"
                // Uncomment the following lines to send an email notification on failure
                // mail to: 'onetenge@gmail.com',
                //     subject: "Failed Deployment: ${currentBuild.fullDisplayName}",
                //     body: "The deployment failed. Please check the Jenkins logs for more details."
            }
        }
    }
}

pipeline {
    agent any

    environment {
        JAVA_HOME = 'C:\\Program Files\\Java\\jdk-11'
        PATH = "${env.PATH};${env.JAVA_HOME}\\bin"
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
        always {
            script {
                echo 'Cleaning up...'
            }
        }
        success {
            script {
                echo "Build was successful!"
                emailext subject: "Successful Deployment: ${currentBuild.fullDisplayName}",
                    body: "The deployment was successful. \n\nBuild details: ${env.BUILD_URL}",
                    to: 'onetenge@gmail.com'
            }
        }
        failure {
            script {
                echo "Build failed!"
                emailext subject: "Failed Deployment: ${currentBuild.fullDisplayName}",
                    body: "The deployment failed. Please check the Jenkins logs for more details. \n\nBuild details: ${env.BUILD_URL}",
                    to: 'onetenge@gmail.com'
            }
        }
    }
}

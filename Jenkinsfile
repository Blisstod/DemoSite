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
                git 'https://github.com/Blisstod/DemoSite.git'  // Ensure the repository URL is correct
            }
        }
        stage('Build') {
            steps {
                script {
                    echo 'Building the project...'
                    bat 'mvn clean install -X'  // Build the project using Maven
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                    bat 'mvn test'  // Run tests using Maven
                }
            }
        }
        stage('Deploy Site') {
            steps {
                script {
                    echo 'Deploying Site...'
                    dir('site') {
                        bat 'start /B mvn spring-boot:run'  // Run the site service in the background
                    }
                }
            }
        }
        stage('Deploy Admin') {
            steps {
                script {
                    echo 'Deploying Admin...'
                    dir('admin') {
                        bat 'start /B mvn spring-boot:run'  // Run the admin service in the background
                    }
                }
            }
        }
        stage('Deploy API') {
            steps {
                script {
                    echo 'Deploying API...'
                    dir('api') {
                        bat 'start /B mvn spring-boot:run'  // Run the API service in the background
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
            }
        }
        failure {
            script {
                echo "Build failed!"
            }
        }
    }
}

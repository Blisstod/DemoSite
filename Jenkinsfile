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
                git 'https://github.com/Blisstod/DemoSite.git'
            }
        }
        stage('Build') {
            steps {
                script {
                    echo 'Building the project...'
                    bat 'mvn clean install -X'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                    bat 'mvn test'
                }
            }
        }
        stage('Deploy Site') {
            steps {
                script {
                    echo 'Deploying Site...'
                    dir('site') {
                        bat 'start /B mvn spring-boot:run'
                    }
                }
            }
        }
        stage('Deploy Admin') {
            steps {
                script {
                    echo 'Deploying Admin...'
                    dir('admin') {
                        bat 'start /B mvn spring-boot:run'
                    }
                }
            }
        }
        stage('Deploy API') {
            steps {
                script {
                    echo 'Deploying API...'
                    dir('api') {
                        bat 'start /B mvn spring-boot:run'
                    }
                }
            }
        }
        stage('Verify Deployment') {
            steps {
                script {
                    echo 'Verifying Site Deployment...'
                    bat 'curl http://localhost:8080'  // Check if site is accessible
                    echo 'Verifying Admin Deployment...'
                    bat 'curl http://localhost:8081'  // Adjust the port if different
                    echo 'Verifying API Deployment...'
                    bat 'curl http://localhost:8082'  // Adjust the port if different
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

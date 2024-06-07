pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Blisstod/DemoSite.git'  // Make sure the repository URL is correct
            }
        }
        stage('Build') {
            steps {
                bat 'mvn clean install -X'  // Build the project using the system-installed Maven
            }
        }
        stage('Test') {
            steps {
                bat 'mvn test'  // Run tests using the system-installed Maven
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def services = ['site', 'admin', 'api']
                    for (service in services) {
                        bat "cd ${service} && mvn spring-boot:run"  // Start services using the system-installed Maven
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

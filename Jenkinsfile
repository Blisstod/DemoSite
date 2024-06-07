pipeline {
    agent any

    environment {
        MAVEN_HOME = tool name: 'Maven 3.6.3', type: 'maven'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-username/CommunityDemo.git'
            }
        }
        stage('Build') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn clean install"
            }
        }
        stage('Test') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn test"
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def services = ['site', 'admin', 'api']
                    for (service in services) {
                        sh "cd ${service} && ${MAVEN_HOME}/bin/mvn spring-boot:run"
                    }
                }
            }
        }
    }

    post {
        success {
            mail to: 'your-email@example.com',
                subject: "Successful Deployment: ${currentBuild.fullDisplayName}",
                body: "The deployment was successful."
        }
        failure {
            mail to: 'your-email@example.com',
                subject: "Failed Deployment: ${currentBuild.fullDisplayName}",
                body: "The deployment failed. Please check the Jenkins logs for more details."
        }
    }
}

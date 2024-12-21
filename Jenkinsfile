pipeline {
    agent any
    tools {
        maven 'Maven3' 
        jdk 'JDK17'
    }
    environment {
        SONAR_HOST_URL = 'http://localhost:9000' // SonarQube server URL
        SONAR_AUTH_TOKEN = credentials('SONARQUBE_AUTH_TOKEN') // Using Jenkins credentials to securely access the token
        SLACK_CHANNEL = '#dev-ops' // Slack channel name
        SLACK_CREDENTIALS = credentials('SLACK_CREDENTIALS') // Jenkins credentials for Slack (Slack Incoming Webhook URL)
    }
    stages {
        // Stage to checkout the code from the Git repository
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/anuradha2603/jenkins_assignment.git'
            }
        }
        // Stage to build the project using Maven
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }
        // Stage to run unit tests and collect code coverage data using JaCoCo
        stage('Unit Tests and Coverage') {
            steps {
                sh 'mvn test'
                jacoco execPattern: 'target/jacoco.exec', classPattern: '**/classes', sourcePattern: '**/src/main/java'
            }
        }
        // Stage to analyze the code quality using SonarQube
        stage('Code Quality Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh "mvn sonar:sonar -Dsonar.host.url=${SONAR_HOST_URL} -Dsonar.login=${SONAR_AUTH_TOKEN} -Dsonar.qualitygate.wait=true"
                }
            }
        }
        // Stage to perform a security scan using Dependency-Check
        stage('Security Scan') {
            steps {
                sh 'dependency-check --scan ./ --format XML --out dependency-check-report.xml'
            }
        }
        // Stage to publish code coverage reports in HTML format
        stage('Publish Reports') {
            steps {
                publishHTML(target: [
                    reportDir: 'target/site/jacoco',
                    reportFiles: 'index.html',
                    keepAll: true,
                    alwaysLinkToLastBuild: true,
                    reportName: 'Code Coverage Report'
                ])
            }
        }
    }

    post {
        // Actions to perform when the build is successful
        success {
            emailext(
                subject: "Build Success: ${currentBuild.fullDisplayName}",
                body: "Good news! The build was successful. Check the build details here: ${BUILD_URL}",
                to: 'anuradha.paswan26@gmail.com'
            )
            slackSend (
                channel: "${SLACK_CHANNEL}",
                color: "good",
                message: "Build Success: ${currentBuild.fullDisplayName} - Check the build details: ${BUILD_URL}",
                tokenCredentialId: "${SLACK_CREDENTIALS}" // Slack credentials for authentication
            )
        }
        // Actions to perform when the build fails
        failure {
            emailext(
                subject: "Build Failed: ${currentBuild.fullDisplayName}",
                body: "Unfortunately, the build failed. Check the build logs: ${BUILD_URL}",
                to: 'anuradha.paswan26@gmail.com'
            )
            slackSend (
                channel: "${SLACK_CHANNEL}",
                color: "danger",
                message: "Build Failed: ${currentBuild.fullDisplayName} - Check the build logs: ${BUILD_URL}",
                tokenCredentialId: "${SLACK_CREDENTIALS}" // Slack credentials for authentication
            )
        }
        // Actions to perform after every build (success or failure)
        always {
            archiveArtifacts artifacts: 'dependency-check-report.xml', fingerprint: true
            junit 'target/surefire-reports/*.xml'
        }
    }
}

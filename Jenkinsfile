pipeline {
    agent any
    tools {
        maven 'Maven3' 
        jdk 'JDK17'
    }
    environment {
        SONAR_HOST_URL = 'http://localhost:9000' // Replace with your SonarQube server URL
        SONAR_AUTH_TOKEN = credentials('SONARQUBE_AUTH_TOKEN') // Using Jenkins credentials to securely access the token
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/anuradha2603/jenkins_assignment.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Unit Tests and Coverage') {
            steps {
                sh 'mvn test'
                jacoco execPattern: 'target/jacoco.exec', classPattern: '**/classes', sourcePattern: '**/src/main/java'
            }
        }
        stage('Code Quality Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh "mvn sonar:sonar -Dsonar.host.url=${SONAR_HOST_URL} -Dsonar.login=${SONAR_AUTH_TOKEN} -Dsonar.qualitygate.wait=true"
                }
            }
        }

        stage('Security Scan') {
            steps {
                sh 'dependency-check --scan ./ --format XML --out dependency-check-report.xml'
            }
        }
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
    success {
        emailext(
            subject: "Build Success: ${currentBuild.fullDisplayName}",
            body: "Good news! The build was successful. Check the build details here: ${BUILD_URL}",
            to: 'anuradha.paswan26@gmail.com'
        )
    }
    failure {
        emailext(
            subject: "Build Failed: ${currentBuild.fullDisplayName}",
            body: "Unfortunately, the build failed. Check the build logs: ${BUILD_URL}",
            to: 'anuradha.paswan26@gmail.com'
        )
    }
    always {
        archiveArtifacts artifacts: 'dependency-check-report.xml', fingerprint: true
        junit 'target/surefire-reports/*.xml'
    }
}

}

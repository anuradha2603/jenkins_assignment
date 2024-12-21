# Scoreme Assignment

The Scoreme Assignment demonstrates a fully automated Jenkins CI/CD pipeline for a simple Java project. The pipeline integrates multiple tools to ensure high-quality, secure, and well-tested code. 

The pipeline is triggered on each commit to the master branch, ensuring continuous integration and continuous delivery with automated feedback on code quality and security. The setup requires Jenkins with necessary plugins (SonarQube, Slack, Email Extension) and appropriate configuration of external tools. This ensures seamless integration of testing, quality checks, and security scans, helping maintain a high standard for the project’s codebase.

# Sample Java Project

This is a simple Java project created for the Jenkins CI/CD pipeline assignment. 

## Features
- A `HelloWorld` application.
- Unit tests for code coverage.
- Configured with Maven for build and dependency management.

## How to Run
1. Compile the project:
   ```bash
   mvn compile

2. Run the tests:
   ```bash
    mvn test

3. Generate code coverage reports:
   ```bash
    mvn verify

The JaCoCo code coverage report will be available at target/site/jacoco/index.html.


# Jenkins CI/CD Pipeline

This repository contains a Jenkins pipeline configured to perform the following CI/CD tasks:
- **Build**: Compile the Java project using Maven.
- **Test**: Run unit tests and calculate code coverage.
- **Code Quality**: Analyze the code using SonarQube for bugs and technical debt.
- **Security**: Scan project dependencies for vulnerabilities using OWASP Dependency-Check.
- **Notifications**: Send build success or failure notifications via email/Slack.

## Setup

### Prerequisites
- Jenkins with the necessary plugins installed (SonarQube, Email Extension, Slack Notification).
- SonarQube server for code quality analysis.
- OWASP Dependency-Check for vulnerability scanning.

### Configuring the Jenkins Pipeline
1. Clone the repository and navigate to the project directory.
2. Ensure that the **Jenkinsfile** is correctly set up for your environment.
3. Configure Jenkins to pull from your Git repository.
4. Set up SonarQube, OWASP Dependency-Check, and other tools as described in the pipeline.

### Running the Pipeline
Once the setup is complete, trigger a build manually or on every commit to the `main` branch. The pipeline will perform the following stages:
1. **Checkout**: Pulls code from the Git repository.
2. **Build**: Builds the project using Maven.
3. **Test and Coverage**: Runs unit tests and generates code coverage reports.
4. **Code Quality**: Analyzes code quality using SonarQube.
5. **Security Scan**: Scans for vulnerabilities using OWASP Dependency-Check.
6. **Reports**: Publishes the reports to Jenkins.

### Notifications
- Email and Slack notifications will be sent on build success or failure.

## Troubleshooting
- Common issues include configuration errors with tools like SonarQube, missing environment variables, and failed tests.

## Conclusion
This Jenkins pipeline helps maintain high-quality, secure, and well-tested code by integrating automated code quality checks, test coverage, and vulnerability scanning.


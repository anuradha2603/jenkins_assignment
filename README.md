# Scoreme Assignment
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
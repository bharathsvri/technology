# CI/CD with Java

## What is CI/CD?
CI/CD (Continuous Integration/Continuous Deployment) automates building, testing, and deploying applications.

## Jenkins Pipeline
```groovy
pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }
        
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        
        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }
        
        stage('Docker Build') {
            steps {
                sh 'docker build -t myapp:${BUILD_NUMBER} .'
            }
        }
        
        stage('Deploy') {
            steps {
                sh 'kubectl set image deployment/myapp myapp=myapp:${BUILD_NUMBER}'
            }
        }
    }
}
```

## GitHub Actions
```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up JDK 17
      uses: actions/setup-java@v3
      with:
        java-version: '17'
        distribution: 'temurin'
    
    - name: Build with Maven
      run: mvn clean compile
    
    - name: Run tests
      run: mvn test
    
    - name: Package
      run: mvn package -DskipTests
    
    - name: Build Docker image
      run: docker build -t myapp:${{ github.sha }} .
    
    - name: Deploy to Kubernetes
      run: |
        kubectl set image deployment/myapp myapp=myapp:${{ github.sha }}
```

## GitLab CI
```yaml
stages:
  - build
  - test
  - package
  - deploy

build:
  stage: build
  image: maven:3.8-openjdk-17
  script:
    - mvn clean compile

test:
  stage: test
  image: maven:3.8-openjdk-17
  script:
    - mvn test
  artifacts:
    reports:
      junit: target/surefire-reports/TEST-*.xml

package:
  stage: package
  image: maven:3.8-openjdk-17
  script:
    - mvn package -DskipTests
  artifacts:
    paths:
      - target/*.jar

deploy:
  stage: deploy
  script:
    - docker build -t myapp:$CI_COMMIT_SHA .
    - kubectl set image deployment/myapp myapp=myapp:$CI_COMMIT_SHA
  only:
    - main
```

## Best Practices
1. Automate everything
2. Run tests in CI
3. Use code quality checks
4. Build Docker images
5. Deploy to staging first
6. Use blue-green deployment
7. Monitor deployments
8. Rollback strategy
9. Security scanning
10. Performance testing


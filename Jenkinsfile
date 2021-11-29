pipeline {
    agent any
    environment {
        COMMIT_HASH = "${sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()}"
        AWS_ID = credentials('aws-id')
        IMG_NAME = "user-service"
        REPO_URL = credentials('service-user')
    }

    // tools {
    //     SonarQube Scanner "Local Sonar Scanner"
    // }

    stages {
        stage('Clean target') {
            steps {
                sh 'mvn clean'
            }
        }
        stage('Build') {
            steps {
                // Run Maven on a Unix agent.
                sh "mvn test package"

            }
        }
        stage('Code Analysis: Sonarqube') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Await Quality Gateway') {
            steps {
                waitForQualityGate abortPipeline: true
            }
        }
    }
    post {
        always {
            sh 'mvn clean'
            sh "docker system prune -f"
        }
    }
}

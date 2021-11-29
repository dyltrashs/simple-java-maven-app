pipeline {
    agent any
    
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
}

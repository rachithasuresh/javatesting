pipeline {
    agent any

    tools {
        // Use the Maven tool configured in Jenkins
        maven 'sonarcube 3.8.7'
    }

    environment {
        // This specifies the SonarQube environment
        SONARQUBE_ENV = 'codinglab'
    }

    stages {
        stage('Checkout') {
            steps {
                // Check out the code from the Git repository
                git 'https://github.com/rachithasuresh/javatesting.git'
            }
        }

        stage('Build') {
            steps {
                // Run Maven to clean the project and compile the code
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Run SonarQube analysis using the Maven Sonar plugin
                withSonarQubeEnv(SONARQUBE_ENV) {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                // Wait for SonarQube Quality Gate to complete
                waitForQualityGate abortPipeline: true
            }
        }
    }

    post {
        failure {
            // Actions to take if the pipeline fails
            echo 'The build failed.'
        }
    }
}

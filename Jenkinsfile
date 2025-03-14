pipeline {
    agent any

    tools {
        jdk 'JDK11'  // Ensure JDK is properly configured
        maven 'Maven3'  // Ensure Maven is properly configured
        sonarQubeScanner 'SonarQube Scanner'  // Ensure SonarQube Scanner is configured properly in Jenkins
        dependencyCheck 'DependencyCheck'  // Ensure DependencyCheck is configured in Jenkins
    }

    environment {
        SONARQUBE = 'SonarQube'  // This can be used for SonarQube server reference in withSonarQubeEnv
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/hariharan-k21/SonarQube-Jenkins.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Ensure SonarQube Scanner is defined properly in Jenkins Global Tool Configuration
                    withSonarQubeEnv('SonarQube') {  // 'SonarQube' must match the SonarQube server configured in Jenkins
                        sh 'sonar-scanner'
                    }
                }
            }
        }

        stage('Dependency-Check Analysis') {
            steps {
                script {
                    // Ensure Dependency-Check is configured and installed in Global Tool Configuration
                    dependencyCheck odcInstallation: 'MyDependencyCheckInstallation', additionalArguments: '-DdependencyCheck.skip=false'
                }
            }
        }
    }

    post {
        always {
            echo 'Build finished'
        }
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed. Please check the logs for details.'
        }
    }
}

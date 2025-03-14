pipeline {
    agent any
    tools {
        jdk 'JDK11'  // Ensure JDK is properly configured
        maven 'Maven3'  // Ensure Maven is properly configured
    }
    environment {
        SONARQUBE = 'SonarQube'  // You have declared this, but not really needed unless used somewhere
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
                    def scannerHome = tool name: 'SonarQube Scanner', type: 'ToolType'  // ToolType is optional
                    withSonarQubeEnv('SonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
        stage('Dependency-Check Analysis') {
            steps {
                // Ensure Dependency-Check is installed and configured under Global Tool Configuration
                dependencyCheck odcInstallation: 'MyDependencyCheckInstallation', additionalArguments: '-DdependencyCheck.skip=false'
            }
        }
    }
    post {
        always {
            echo 'Build finished'
        }
    }
}

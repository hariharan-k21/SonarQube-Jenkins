pipeline {
    agent any
    tools {
        jdk 'JDK11'  // Ensure JDK is properly configured in Jenkins Global Tool Configuration
        maven 'Maven3'  // Ensure Maven is properly configured in Jenkins Global Tool Configuration
    }
    environment {
        SONARQUBE = 'SonarQube'  // Ensure this matches your SonarQube server name in Jenkins config
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
                    def scannerHome = tool name: 'SonarQube Scanner', type: 'Tool'
                    withSonarQubeEnv('SonarQube') {  // Ensure 'SonarQube' matches the server name in Jenkins config
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

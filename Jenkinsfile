pipeline {
    agent any

    tools {
        jdk 'JDK11'  // Ensure JDK is properly configured in Global Tool Configuration
        maven 'Maven3'  // Ensure Maven is properly configured in Global Tool Configuration
    }

    environment {
        SONARQUBE = 'SonarQube'  // SonarQube server name
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/hariharan-k21/SonarQube-Jenkins.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'  // Build the project using Maven
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Ensure SonarQube Scanner is defined properly in Jenkins Global Tool Configuration
                    def scannerHome = tool name: 'SonarQube Scanner', type: 'Tool'
                    withSonarQubeEnv(SONARQUBE) {
                        sh "${scannerHome}/bin/sonar-scanner"  // Run SonarQube analysis
                    }
                }
            }
        }

        stage('Dependency-Check Analysis') {
            steps {
                // Use the correct parameter 'odcInstallation' for Dependency-Check
                dependencyCheck odcInstallation: 'MyDependencyCheckInstallation', additionalArguments: '-DdependencyCheck.skip=false'
            }
        }
    }

    post {
        always {
            echo 'Build finished'  // This will run after all stages, regardless of success or failure
        }
    }
}

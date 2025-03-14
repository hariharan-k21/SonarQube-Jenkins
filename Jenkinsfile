pipeline {
    agent any
    tools {
        jdk 'JDK11'
        maven 'Maven3'
    }
    environment {
        SONARQUBE = 'SonarQube'
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
                    def scannerHome = tool 'SonarQube Scanner'
                    withSonarQubeEnv('SonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
        stage('Dependency-Check Analysis') {
            steps {
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

pipeline {
    agent any
    tools {
        jdk 'JDK11'  // JDK version you have installed
        maven 'Maven3'  // Maven configuration
    }
    environment {
        SONARQUBE = 'SonarQube'  // SonarQube server name as configured in Jenkins
    }
    stages {
        stage('Checkout SCM') {
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
                    withSonarQubeEnv('SonarQube') {
                        sh '''
                            sonar-scanner \
                            -Dsonar.projectKey=sonar-example \
                            -Dsonar.host.url=http://your-sonarqube-server-url \
                            -Dsonar.login=your-sonarqube-token
                        '''
                    }
                }
            }
        }
        stage('Dependency-Check Analysis') {
            steps {
                echo 'Running Dependency-Check analysis...'
            }
        }
    }
    post {
        always {
            echo 'Build finished.'
        }
    }
}

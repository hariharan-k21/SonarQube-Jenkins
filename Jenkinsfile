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
                        // Running sonar-scanner with the right environment
                        sh '''
                            sonar-scanner \
                            -Dsonar.projectKey=jenkins \
                            -Dsonar.host.url=http://13.127.204.39:9000/ \
                            -Dsonar.login=jenkins
                        '''
                    }
                }
            }
        }
        stage('Dependency-Check Analysis') {
            steps {
                script {
                    // Run Dependency-Check analysis with Maven plugin
                    echo 'Running Dependency-Check analysis...'
                    // Assuming you have the dependency-check plugin set up
                    sh 'mvn org.owasp:dependency-check-maven:check'
                }
            }
        }
    }
    post {
        always {
            echo 'Build finished.'
        }
    }
}

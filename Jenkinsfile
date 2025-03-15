pipeline {
    agent any
    tools {
        jdk 'JDK11'
        maven 'Maven3'
    }
    environment {
        SONARQUBE = 'SonarQube'  // SonarQube server name as configured in Jenkins
        SONAR_TOKEN = credentials('sonar-token-id')  // Use Jenkins credentials for security
    }
    stages {
        stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/hariharan-k21/SonarQube-Jenkins.git'
            }
        }
        stage('Build') {
            steps {
                sh '''
                    export -Dorg.jenkinsci.plugins.durabletask.BourneShellScript.HEARTBEAT_CHECK_INTERVAL=86400
                    mvn clean install
                '''
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('SonarQube') {
                        sh '''
                            export -Dorg.jenkinsci.plugins.durabletask.BourneShellScript.HEARTBEAT_CHECK_INTERVAL=86400
                            sonar-scanner \
                            -Dsonar.projectKey=jenkins \
                            -Dsonar.host.url=http://13.127.204.39:9000 \
                            -Dsonar.login=$SONAR_TOKEN
                        '''
                    }
                }
            }
        }
        stage('Dependency-Check Analysis') {
            steps {
                script {
                    echo 'Running Dependency-Check analysis...'
                    sh '''
                        export -Dorg.jenkinsci.plugins.durabletask.BourneShellScript.HEARTBEAT_CHECK_INTERVAL=86400
                        mvn org.owasp:dependency-check-maven:check
                    '''
                }
            }
        }
    }
    post {
        success {
            echo 'Build and analysis completed successfully.'
        }
        failure {
            echo 'Build failed. Please check the logs for details.'
        }
        always {
            echo 'Build finished.'
        }
    }
}

pipeline {
    agent any
    tools {
        jdk 'JDK11'
        maven 'Maven3'
    }
    environment {
        SONARQUBE = 'SonarQube'  // SonarQube server name as configured in Jenkins
        SONAR_TOKEN = 'sqa_319b983f8b82055e5a3f6d1e5a7d2c65e8069cc5'  // Replace with the token you generated
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
                    export -D org.jenkinsci.plugins.durabletask.BourneShellScript.HEARTBEAT_CHECK_INTERVAL=86400
                    mvn clean install
                '''
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('SonarQube') {
                        sh '''
                            export -D org.jenkinsci.plugins.durabletask.BourneShellScript.HEARTBEAT_CHECK_INTERVAL=86400
                            docker run --rm \
                            -v $(pwd):/usr/src \
                            sonarsource/sonar-scanner-cli:latest \
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
                    // Run Dependency-Check analysis with Maven plugin
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
        always {
            echo 'Build finished.'
        }
    }
}

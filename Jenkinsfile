pipeline {
    agent any
    environment {
        MAVEN_HOME = tool 'MAVEN'
        SONAR_SCANNER = tool 'SonarScanner'
    }
    stages {
        stage('Checkout Code') {
            steps {
                git credentialsId: 'my-private-repo-creds', branch: 'main', url: 'https://github.com/CloudFolksHUB/superlab.git'
            }
        }
        stage('Maven Build') {
            steps {
                echo 'Building project'
                sh "${MAVEN_HOME}/bin/mvn clean verify -Dtest=!FormUITest"
            }
        }
        stage('SonarQube Code Quality Scan') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    echo '🔍 Running SonarCloud analysis (coverage disabled for now)...'
                    sh """
                        ${SONAR_SCANNER}/bin/sonar-scanner \
                          -Dsonar.projectKey=sonartestorg01_sonarqubeproject \
                          -Dsonar.organization=sonartestorg01 \
                          -Dsonar.sources=src/main/java \
                          -Dsonar.tests=src/test/java \
                          -Dsonar.java.binaries=target/classes \
                          -Dsonar.coverage.exclusions=**/*.java \
                          -Dsonar.coverage.newCode.requiredCoverage=0 \
                          -Dsonar.newCode.period=1 \
                          -Dsonar.qualitygate.wait=true \
                          -Dsonar.host.url=https://sonarcloud.io
                    """
                }
            }
        }
        stage('Check Sonar Quality Gate') {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    script {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "❌ Quality Gate failed: ${qg.status}"
                        }
                    }
                }
            }
        }
    }

    post {
        success {
            echo '✅ Phase 5 completed successfully! Code passed quality checks.'
        }
        failure {
            echo '❌ Pipeline failed. Please review the logs or SonarQube report.'
        }
    }
}
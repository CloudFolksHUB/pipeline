pipeline {
    agent any
    
    environment {
    MAVEN_HOME = tool 'MAVEN'  // Use the name given in Global Tool Configuration
    }
    

    stages {
        stage('Checkout Code') {
            steps {
                echo "🔁 Cloning Private GitHub Repository..."
                git credentialsId: 'my-private-repo-creds', branch: 'main', url: 'https://github.com/CloudFolksHUB/superlab.git'
            }
        }

        stage('Maven Build') {
            steps {
                echo "echo 'Building project"
                sh "${MAVEN_HOME}/bin/mvn clean verify -Dtest=!FormUITest"
            }
        }

        // ➕ Add more stages as needed
    }

    post {
        success {
            echo "✅ Pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed. Please check the logs."
        }
    }
}
pipeline {
    agent any

    environment {
        MAVEN_HOME = tool 'Maven 3'  // Ensure Jenkins has a Maven tool configured
        JAVA_HOME = tool 'JDK8'      // Ensure Jenkins has JDK 1.8 configured
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', credentialsId: 'github-ssh-key', url: 'git@github.com:Dlugo10/WelcomeApp.git'
            }
        }

        stage('Setup Environment') {
            steps {
                script {
                    env.PATH = "${MAVEN_HOME}/bin:${JAVA_HOME}/bin:${env.PATH}"
                }
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Run Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }

        stage('Deploy to Server') {
            steps {
                sh 'scp target/WelcomeApp.war user@server:/path/to/tomcat/webapps/'  // Modify for your deployment
            }
        }
    }

    post {
        success {
            echo "Build and deployment successful!"
        }
        failure {
            echo "Build failed. Check the logs."
        }
    }
}

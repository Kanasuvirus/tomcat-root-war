pipeline {
    agent { label 'slave1' }  // Adjust if needed
    environment {
        TOMCAT_USER = 'admin'  // Change to your Tomcat username
        TOMCAT_PASSWORD = 'admin'  // Change to your Tomcat password
        TOMCAT_URL = 'http://54.234.238.119:8080'  // Update with your Tomcat URL
    }
    
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/Kanasuvirus/tomcat-root-war.git'
            }
        }
        
        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        
        stage('Deploy to Tomcat') {
            steps {
                script {
                    def warFile = 'target/ROOT.war'  // Ensure this matches your build output
                    def deployUrl = "${TOMCAT_URL}/manager/text/deploy?path=/java_webapp&update=true"

                    sh """
                        curl -u ${TOMCAT_USER}:${TOMCAT_PASSWORD} --upload-file ${warFile} ${deployUrl}
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Build & Deployment Successful!"
        }
        failure {
            echo "❌ Build Failed! Check logs."
        }
    }
}


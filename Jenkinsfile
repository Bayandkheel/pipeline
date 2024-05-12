pipeline {
    agent any

    tools {
        maven 'Maven 3.6.3' // Ensure Maven is configured in Jenkins global tools with the name 'Maven 3.6.3'
        jdk 'Java 11'       // Ensure JDK is configured in Jenkins global tools with the name 'Java 11'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Unit and Integration Tests') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml' // Publish JUnit test result report
                }
            }
        }
        
        stage('Code Analysis') {
            steps {
                sh 'mvn sonar:sonar'
            }
        }
        
        stage('Security Scan') {
            steps {
                sh 'mvn org.owasp:dependency-check-maven:check'
            }
            post {
                always {
                    // Configure to send email if this step is important for notification
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("myapp:${env.BUILD_ID}")
                }
            }
        }
        
        stage('Integration Tests on Staging') {
            steps {
                // Simulate staging tests or configure deployment to staging
                echo 'Run integration tests in staging environment'
            }
        }
        
        stage('Deploy to Production') {
            steps {
                echo 'Deploying to AWS EC2 instance'
                // Add deployment scripts here
            }
        }
    }
    
    post {
        always {
            // Email notifications can be configured here
            emailext (
                subject: "Build ${currentBuild.fullDisplayName}",
                body: """<p>Build Result: ${currentBuild.currentResult}</p>
                         <p>Check console output at <a href='${buildUrl}'>${buildUrl}</a></p>""",
                to: 'email@example.com'
            )
        }
    }
}

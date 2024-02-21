pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'Java11'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/chikababy/Jenkinscicdfinal.git'
            }
        }
        
        stage('Build') {
            steps {
                // Commands to build your project
                sh 'mvn clean package'
            }
        }
        
        stage('Test') {
            steps {
                // Commands to run tests
                sh 'mvn test'
                stash(name: 'packaged_code', includes: 'target/*.war')
            }
        }
        
        stage('Deploy to Tomcat') {
            agent {
                label 'tomcat'
            }
            steps {
                // Commands to deploy your application
                unstash 'packaged_code'
                sh "sudo rm -rf ~/apache*/webapps/*.war"
                sh "sudo mv target/*.war ~/apache*/webapps/"
                sh "sudo ~/apache*/bin/shutdown.sh && sudo ~/apache*/bin/startup.sh"
            }
        }
    }

    post {
        always {
            emailtext body: 'Check console output at $BUILD_URL to view the results.',
                     subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!',
                     to: 'okorjichika.co@gmail.com'
        }
    }
}

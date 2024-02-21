pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'Java11'
    }

    stages {
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
            }
        }
        stage('Deploy') {
            steps {
                // Commands to deploy your application
                sh 'cp target/WebAppCal-0.0.6.war /path/to/tomcat/webapps/'
            }
        }
    }
}

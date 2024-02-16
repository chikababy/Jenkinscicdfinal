pipeline {
     label 'master'

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
                sh 'cp target/my-application.war /path/to/tomcat/webapps/'
            }
        }
    }
}

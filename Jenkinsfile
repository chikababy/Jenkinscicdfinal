pipeline {
    agent {
        label 'node1'
    }

    stages {
        stage('SCM Checkout') {
            steps {
                script {
                    git tool: 'Default', credentialsId: 'git-cred', url: 'https://github.com/chikababy/Jenkinscicdfinal.git'
                }
            }
        }

        stage('Mvn Package') {
            steps {
                script {
                    def mvnHome = tool name: 'apache-maven-3.9.6', type: 'maven'
                    def mvnCMD = "${mvnHome}/opt/mvn"
                    
                    // Build and test a single step
                    sh "${mvnCMD} clean package test"
                    stash(name: "Jenkinscicdfinal", includes: "target/*.war")
                }
            }
        }

        stage('Deploy Application') {
            agent {
                label 'tomcat'
            }
            steps {
                echo "Deploying the application"
                script {
                    // Create the target directory if it doesn't exist
                    sh "sudo mkdir -p /home/centos/apache-tomcat-7.0.94/webapps/"

                    // Remove existing WAR files
                    sh "sudo rm -rf /home/centos/apache-tomcat-7.0.94/webapps/*.war"

                    unstash "Jenkinscicdfinal"

                    // Move the WAR file to the target directory
                    sh "sudo mv target/*.war /home/centos/apache-tomcat-7.0.94/webapps/"

                    // Reload systemd daemon
                    sh "sudo systemctl daemon-reload"

                    // Restart Tomcat
                    sh "sudo /home/centos/apache-tomcat-7.0.94/bin/startup.sh"
                }
            }
        }
    }
}

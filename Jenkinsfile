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
                    def mvnCMD = "/opt/apache-maven-3.9.6/bin/mvn"
                    
                    // Build and test a single step
                    sh "${mvnCMD} clean package test"
                    stash(name: "Jenkinscicdfinal", includes: "target/*.war")
                }
            }
        }

        stage('Deploy Application') {
            agent {
                label 'node1'
            }
            steps {
                echo "Deploying the application"
                script {
                    // Download and install Apache Tomcat
                    def tomcatUrl = 'https://dlcdn.apache.org/tomcat/tomcat-8/v8.5.99/bin/apache-tomcat-8.5.99.tar.gz'
                    sh "wget -q -O tomcat.tar.gz ${tomcatUrl}"
                    sh "sudo tar -xzf tomcat.tar.gz -C /opt"

                    // Create the target directory if it doesn't exist
                    sh "sudo mkdir -p /home/centos/apache-tomcat-7.0.94/webapps/"

                    // Remove existing WAR files
                    sh "sudo rm -rf /home/centos/apache-tomcat-7.0.94/webapps/*.war"

                    // Restore stashed WAR file
                    unstash "Jenkinscicdfinal"

                    // Move the WAR file to the target directory
                    sh "sudo mv target/*.war /home/centos/apache-tomcat-7.0.94/webapps/"

                    // Restart Tomcat
                    sh "sudo /opt/apache-tomcat-8.5.99/bin/startup.sh"
                }
            }
        }
    }
}

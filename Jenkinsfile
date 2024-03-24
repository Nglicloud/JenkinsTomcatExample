pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout code from GitHub repository
                git branch: 'master', url: 'https://github.com/Nglicloud/JenkinsTomcatExample.git'
            }
        }
        
        stage('Build') {
            steps {
                // Build using Maven
                bat 'mvn clean package'
            }
        }
        
        stage('Publish to Artifactory') {
            steps {
                // Publish artifact to JFrog Artifactory
                script {
                    def server = Artifactory.server 'Jfrog-Maven'
                    def buildInfo = Artifactory.newBuildInfo()
                    server.publishBuildInfo buildInfo
                }
            }
        }
        
        stage('Deploy to Tomcat') {
            steps {
                // Deploy artifact to Tomcat
                script {
                    def tomcatUrl = 'http://51.20.86.162:8080/'
                    def warFilePath = '/home/ec2-user/tomcat/webapps'
                    bat """
                    powershell -Command \"\$user='tomcat'; \$pass='s3cret'; \$cred = New-Object System.Management.Automation.PSCredential(\$user, (\$pass | ConvertTo-SecureString -AsPlainText -Force)); Invoke-WebRequest -Uri '${tomcatUrl}/manager/text/deploy?path=/webapps' -Method Post -Credential \$cred -InFile '${warFilePath}'\"
                    """

                }
            }
        }
    }
}

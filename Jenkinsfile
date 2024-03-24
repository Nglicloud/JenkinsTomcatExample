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
                sh 'clean install'
            }
        }
        
        stage('Publish to Artifactory') {
            steps {
                // Publish artifact to JFrog Artifactory
                script {
                    def server = Artifactory.server 'http://51.20.86.162:8082/artifactory'
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
                    sh "curl -u tomcat:s3cret -T ${warFilePath} ${tomcatUrl}/manager/text/deploy?path=/webapps"
                }
            }
        }
    }
}

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
                sshPublisher(publishers: [sshPublisherDesc(configName: 'tomcat', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: 'target', sourceFiles: 'target/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
    }
}

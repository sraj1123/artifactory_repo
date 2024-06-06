pipeline {
    agent any

    // Define tools to be installed
    tools {
        // Define Maven tool installation
        maven 'maven'
        sonarqubeScanner 'SonarQubeScanner'
    }
 
    stages {
        stage('Git Clone') {
            steps {
                // Get code from a GitHub repository
                git branch: 'master', url: 'https://github.com/sraj1123/artifactory_repo.git'
            }
        }

        stage('Maven Build') {
            steps {
                script {
                    // Build Maven Code
                    sh "mvn clean install"
                }
            }
        }

        stage('Upload to Artifactory') {
            steps {
                script {
                    def server = Artifactory.newServer url: 'http://172.16.7.108:8082/artifactory/HP/', credentialsId: 'JFrog'
                    def uploadSpec = """{
                        "files": [
                            {
                                "pattern": "module-a/target/*.jar",
                                "target": "HP/"
                            }
                        ]
                    }"""
                    server.upload(uploadSpec)
                }
            }
        }
    }
}

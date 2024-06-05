pipeline {
    agent any
    tools {
        maven "maven"
        //Local environment variable for maven dependency and its location
    }
    stages {
            stage('Git Clone') {
                steps {
                    // Get code from a GitHub repository
                    git branch: 'master', url: 'https://github.com/sraj1123/FinalBCI'
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
            stage('Sonar Scan') {
                steps {
                    script {
                        //Run Sonarqube Scan
                    withSonarQubeEnv(credentialsId: 'SonarQube_Token') {
                    sh 'mvn sonar:sonar -Dsonar.projectName=Test2 -Dsonar.projectKey=Test2'
                        }
                    }
                }
            }
            stage("Uploading to JFrog Artifactory"){
                steps{
                    rtUpload(
                        serverId: "Jfrog",
                        spec: '''{
                            "files":[{
                                "pattern": "module-a/target/*.jar",
                                "target": "example-repo-local"
                            }]
                        }
                        ''',
    
                        )
                        }
                }                
            stage('Archive Artivact') {
                steps {
                    //Archive Artifact
                    archiveArtifacts artifacts: 'module-a/target/*.jar', followSymlinks: false
                }
            }					   
        }
}

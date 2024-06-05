pipeline {
    agent {
        node{
            label 'maven-server'
            //Remote build server node
        }
    }
    tools {
        maven "mvn"
        //Local environment variable for maven dependency and its location
    }
    stages {
            stage('Git Clone') {
                steps {
                    // Get code from a GitHub repository
                    git branch: 'main', url: 'https://github.com/sraj1123/FinalBCI'
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
                        serverId: "jfrog",
                        spec: '''{
                            "files":[{
                                "pattern": "module-a/target/*.jar",
                                "target": "be-branch"
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
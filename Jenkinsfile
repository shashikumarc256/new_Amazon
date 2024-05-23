pipeline {
    agent any

    tools{
        maven 'maven'
    }

    environment {
        ARTIFACTORY_SERVER = 'jfrog-artifactory-instance' // Artifactory server ID configured in Jenkins
        ARTIFACTORY_REPO = 'artifactory-build-info'               // Target repository in Artifactory
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/shashikumarc256/new_Amazon.git', branch: 'master'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('jfrog') {
            steps {
                script {
                    // Define the Artifactory server
                    def server = Artifactory.server(ARTIFACTORY_SERVER)
                    
                    // Define the upload spec
                    def uploadSpec = """{
                        "files": [{
                            "pattern": "target/*.jar",
                            "target": "${ARTIFACTORY_REPO}/"
                        }]
                    }"""
                    
                    // Upload artifacts to Artifactory
                    server.upload(uploadSpec)
                }
            }
        }
    }

}

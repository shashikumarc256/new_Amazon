pipeline {
    agent any

    tools{
        maven 'maven'
    }

    environment {
        ARTIFACTORY_SERVER_ID = 'jfrog-artifactory-instance'
        ARTIFACTORY_REPO = 'artifactory-build-info'
        ARTIFACTORY_CREDENTIALS_ID = 'jfrog-artifactory-instance'
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

        stage('Upload to Artifactory') {
            steps {
                script {
                    def server = Artifactory.server(ARTIFACTORY_SERVER_ID)

                    def uploadSpec = """{
                        "files": [{
                            "pattern": "target/*.jar",
                            "target": "${ARTIFACTORY_REPO}/"
                        }]
                    }"""

                    server.upload(uploadSpec)
                }
            }
        }

        stage('Publish Build Info') {
            steps {
                script {
                    def server = Artifactory.server(ARTIFACTORY_SERVER_ID)

                    def buildInfo = Artifactory.newBuildInfo()
                    buildInfo.env.capture = true
                    
                    server.publishBuildInfo(buildInfo)
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}

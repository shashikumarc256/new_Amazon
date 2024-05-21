pipeline{
    agent any
    tools{
        maven 'maven'
    }
    environment{
        AZURE_CREDENTIALS_ID='azure_principal_id'
    }

    stages{
        stage('clone'){
            steps{
                     git branch : 'master', url: 'https://github.com/teambhoomi/Amazon.git'
            }
        }
    

       stage('terraform init') {
           steps{
               script{
                   withCredentials([azureServicePrincipal('azure_principal_id')]) {
                                sh 'az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID'


                       dir('Terraform'){
                                sh 'terraform --version'
                                sh 'terraform init'
                       }
                               
                     }

               }
           }
       }
        stage('terraform plan') {
            steps {
                script {
                    
                        sh 'pwd'
                        sh 'ls -l'
                        sh 'terraform plan'
                    
                     
                }
            }
        }
        stage('terraform apply') {
            steps {
                   script{
                    dir('Terraform') {
                        sh 'pwd'
                        sh 'ls -l'
                         sh 'terraform apply -auto-approve'
                    }
                }
            }
        }

        stage('sonar'){
            steps{
                withSonarQubeEnv('sonarqube') {
                        sh 'mvn clean package sonar:sonar'
                    }

                }

        }
        stage('build') {
            steps{
                sh 'mvn clean install'
            }
        }

        stage('test'){
            steps{
                sh 'mvn clean -DskipTests'
            }
        }

    }
}


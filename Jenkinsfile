pipeline {
    agent any
    options {
        timeout(time: 20, unit: 'MINUTES')
    }
    stages{
        // NPM dependencies
        stage('pull npm dependencies') {
            steps {
                sh 'npm install'
            }
        }
       stage('build Docker Image') {
            steps {
                script {
                    // build image
                    docker.build("335871625378.dkr.ecr.eu-west-2.amazonaws.com/netflix-job:latest")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 335871625378.dkr.ecr.eu-west-2.amazonaws.com/netflix-job:latest'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix-app', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https://987731527829.dkr.ecr.us-east-2.amazonaws.com/netflix-job', 'ecr:us-east-2:Chinwe-ECR') {
                    // build image
                    def myImage = docker.build("987731527829.dkr.ecr.us-east-2.amazonaws.com/netflix-job:latest")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}

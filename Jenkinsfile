pipeline {
    agent {
        label 'docker-slave'
    }
    stages {
        stage('Code Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/kallakruparaju/Jenkins-Task'
            }
        }
        stage('CI') {
            steps {
                sh 'python3 $WORKSPACE/cal_app/TestCalculator.py'
            }
        }
        stage('CD') {
            environment {
                STACK_NAME = 'sam-app-CD-stage'
                S3_BUCKET = 'aws-sam-cli-managed-default-samclisourcebucket-1nq2jloczupzn'
            }
            steps {
                withAWS(credentials: 'sam-jenkins-credentials', region: 'ap-south-1') {
                    sh 'sam deploy --stack-name $STACK_NAME -t template.yaml --s3-bucket $S3_BUCKET --capabilities CAPABILITY_IAM'
                }
            }
        }
    }
}

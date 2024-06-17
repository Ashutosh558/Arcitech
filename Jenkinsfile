pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Ashutosh558/Arcitech.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                // Add your test commands here
                echo 'Running tests...'
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    // Transfer application files to EC2 instance
                    sh '''
                    scp -i $EC2_KEY -r . $EC2_USER@$EC2_HOST:$APP_DIR
                    ssh -i $EC2_KEY $EC2_USER@$EC2_HOST <<EOF
                    sudo systemctl restart web-app
                    EOF
                    '''
                }
            }
        }

        stage('Update S3 Bucket') {
            steps {
                script {
                    // Upload static files to S3 bucket
                    sh '''
                    aws s3 sync static/ s3://$S3_BUCKET/static/
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            deleteDir()
        }
    }
}

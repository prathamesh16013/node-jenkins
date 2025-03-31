pipeline {
    agent any

    environment {
        EC2_USER = 'ec2-user'
        EC2_HOST = 'your-ec2-ip'
        APP_DIR = '/home/ec2-user/app'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/prathamesh16013/node-jenkins.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build Application') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['your-ssh-credential-id']) {
                    sh """
                        scp -r * ${EC2_USER}@${EC2_HOST}:${APP_DIR}
                        ssh ${EC2_USER}@${EC2_HOST} 'cd ${APP_DIR} && npm install && pm2 restart all'
                    """
                }
            }
        }
    }
}


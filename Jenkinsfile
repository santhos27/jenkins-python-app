pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/your-username/jenkins-python-app.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'python test_app.py'
            }
        }
        stage('Build and Run') {
            steps {
                sh 'nohup python app.py &'
            }
        }
    }
}


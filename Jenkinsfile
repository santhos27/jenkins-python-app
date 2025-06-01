pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                sh 'python3 --version'
                sh 'pip --version'
                git 'https://github.com/santhos27/jenkins-python-app.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh '''
                ls
                if [ -d "pyapp" ]; then
                  rm -rf pyapp
                fi
                python3 -m venv pyapp
                    . venv/bin/activate
                    ls
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }
        stage('Run Tests') {
            steps {
                sh '''
                . venv/bin/activate
                python3 test_app.py
                '''
            }
        }
        stage('Build and Run') {
            steps {
                sh'''
                . venv/bin/activate
                python3 app.py &
                for i in {1..10}; do
                   nc -z 127.0.0.1 5000 && break
                   echo "Waiting for app..."
                   sleep 1
                done
                curl 127.0.0.1:5000
                '''
            }
        }
    }
}

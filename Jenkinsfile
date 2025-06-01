pipeline {
    agent { label 'local_kai' }  // this should match the slave's label
    environment {
        IMAGE_NAME = 'santhoskumardocker/flaskapp' // change to your DockerHub username/repo
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Clone') {
            steps {
                sh 'whoami'
                sh 'python3 --version'
                sh 'pip --version'
                git 'https://github.com/santhos27/jenkins-python-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                ls
                if [ -d "flaskapp" ]; then
                  rm -rf flaskapp
                fi
                python3 -m venv flaskapp
                . flaskapp/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                . flaskapp/bin/activate
                python3 test_app.py
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $IMAGE_NAME:$IMAGE_TAG .
                '''
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    docker push $IMAGE_NAME:$IMAGE_TAG
                    '''
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker --version
                docker ps -a
                docker rm -f flask-app || true
                docker run -d --name flask-app -p 5003:5002 $IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }
    }
}


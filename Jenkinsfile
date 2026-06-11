pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    }

    stages {

        stage('Debug Docker') {
    steps {
        sh '''
        whoami
        echo $PATH
        type docker
        which docker
        ls -l $(which docker)
        /usr/bin/docker --version
        docker --version
        '''
    }
}

        stage('Build Django Image') {
            steps {
                sh '/usr/bin/docker build -t adityaisnomore/django_app:latest .'
            }
        }

        stage('Build Nginx Image') {
            steps {
                sh '/usr/bin/docker build -t adityaisnomore/nginx:latest ./nginx'
            }
        }

        stage('Docker Login') {
            steps {
                sh '''
        echo $DOCKERHUB_CREDENTIALS_PSW | /usr/bin/docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
        '''
            }
        }

        stage('Push Images') {
            steps {
                sh '''
                docker push adityaisnomore/django_app:latest
                docker push adityaisnomore/nginx:latest
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                cd /home/ubuntu/project/django-notes-app
                docker-compose pull
                docker-compose up -d
                '''
            }
        }
    }
}

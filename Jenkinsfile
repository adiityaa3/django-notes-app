pipeline {
agent any

environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
}

stages {

    stage('Clone') {
        steps {
            git branch: 'main',
                url: 'https://github.com/adiityaa3/django-notes-app.git'
        }
    }

    stage('Build Django Image') {
        steps {
            sh 'docker build -t adityaisnomore/django_app:latest .'
        }
    }

    stage('Build Nginx Image') {
        steps {
            sh 'docker build -t adityaisnomore/nginx:latest ./nginx'
        }
    }

    stage('Docker Login') {
        steps {
            sh '''
            echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
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
            docker-compose pull
            docker-compose up -d
            '''
        }
    }
}

}

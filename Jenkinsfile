pipeline {

```
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
            sh 'docker build -t adityaisnomore/django-notes-app:latest .'
        }
    }

    stage('Build Nginx Image') {
        steps {
            sh 'docker build -t adityaisnomore/notes-nginx:latest ./nginx'
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
            docker push adityaisnomore/django-notes-app:latest
            docker push adityaisnomore/notes-nginx:latest
            '''
        }
    }

    stage('Deploy') {
        steps {
            sh '''
            docker-compose down
            docker-compose pull
            docker-compose up -d
            '''
        }
    }
}
```

}

pipeline {
    agent any

    environment {
        REPO_URL    = "https://github.com/megh212/django-notes-app.git"
        REPO_BRANCH = "main"
        IMAGE_NAME  = "megh212/django-notes-app-django"
        IMAGE_TAG   = "local"
    }

    stages {
        stage('Pull') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: "*/${REPO_BRANCH}"]],
                    userRemoteConfigs: [[url: REPO_URL]]
                ])
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
            }
        }

        stage('Test') {
            steps {
                sh 'docker run --rm ${IMAGE_NAME}:${IMAGE_TAG} python manage.py test'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker compose down || true'
                sh 'docker compose up -d --build'
            }
        }
    }

    post {
        always {
            sh 'docker image prune -f || true'
        }
    }
}

pipeline {
    agent none
    stages {
        stage('test') {
            agent { docker { image 'python:3.10' } }
            steps {
                sh '''
                    pwd
                    python -m venv .venv
                    . .venv/bin/activate
                    pip install -r requirements.txt
                    python manage.py test
                    '''
            }
        }
        stage('build') {
            agent any
            steps {
                sh 'docker build . -t aleksandrbabuskin/django_demo:${GIT_COMMIT} -t aleksandrbabuskin/django_demo:latest'
            }
        }
        stage('push') {
            agent any
            steps {
                withCredentials([usernamePassword(credentialsId: 'Docker', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'docker login -u ${USERNAME} -p ${PASSWORD}'
                    sh 'docker push aleksandrbabuskin/django_demo:${GIT_COMMIT}'
                    sh 'docker push aleksandrbabuskin/django_demo:latest'
                }
            }
        }
    }
}